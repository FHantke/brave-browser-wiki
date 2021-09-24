# A Simple(*) Guide to `SimpleURLLoader`
(*) not guaranteed, actual simplicity may vary

## Getting Started

`SimpleURLLoader` can upload or download anything accessible via HTTP or HTTPS, from anywhere in Chromium that has access to `/services/network/`. You have full control over the HTTP headers that are sent in the request, and you have full access to all the headers and data received in the response.

Here is the absolute minimum you will need to do:

 1. Decide which `URLLoaderFactory` to use to send your request. There are two likely choices, one tied to the user profile, and one (more restricted) system-wide. More on this below.
 1. Create a `network::ResourceRequest` object containing information about the request, like the URL you're accessing, the HTTP verb you're using, the HTTP headers you're including, and other options that control how the request is sent.
 1. Create a `net::NetworkTrafficAnnotationTag` object containing human-readable metadata about the request, like the name of the subsystem that will send the request, what conditions trigger the request, and what kind of data will be transferred.
 1. Create a `network::SimpleURLLoader` object and pass it those two objects (resource request and traffic annotation).
 1. For HTTP `POST` requests, attach the data to send in the request body.
 1. Call a method on the `SimpleURLLoader` object to initiate the network request, passing the `URLLoaderFactory` and a callback function that `SimpleURLLoader` will call when the request is complete.

There are several download methods on `SimpleURLLoader`. You probably want one of these:

 * `DownloadToStringOfUnboundedSizeUntilCrashAndDie` is the most common. It downloads a resource of any size and calls your callback function when it's done.
 * `DownloadToString` downloads a small resource (up to 1 `MB`, although you can specify an exact size limit below that) and calls your callback function when it's done. If the resource ends up being larger than the limit, it will call your callback function with an error code.
 * `DownloadHeadersOnly` initiates a network request but ignores any response body, then calls your callback function with the HTTP headers it received.

There are other methods to download to a named file, download to an automatically generated temporary file, or download as a stream. See `simple_url_loader.h` for details.

## Creating The `ResourceRequest`

Include `resource_request.h` at the top of your `.cc` file:

```
#include "services/network/public/cpp/resource_request.h"
```

Create your `ResourceRequest` object:

```
auto resource_request = std::make_unique<network::ResourceRequest>();
```

Set the URL you want to access (this should be a `GURL` object):

```
resource_request->url = target_url;
```

***

_(recommended)_ Set the HTTP verb you want to use (probably `"GET"` or `"POST"`, although all HTTP verbs are supported). `"GET"` is the default verb.

```
resource_request->method = "GET";
```

***

_(recommended)_ Set the load flags (these are `int` values and are meant to be OR'd together with `|`):

```
resource_request->load_flags = net::LOAD_DO_NOT_SEND_COOKIES |
  net::LOAD_DO_NOT_SAVE_COOKIES;
```

If you do not set any load flags, the default behavior will act like a browser, _i.e._ it will respect the HTTP cache, respect the user's proxy settings, send any stored user cookies to the target URL, and update the cookie store and HTTP cache afterwards if the network request is successful. This has obvious privacy implications.

Common load_flags:

 - `net::LOAD_DO_NOT_SEND_COOKIES`
 - `net::LOAD_DO_NOT_SAVE_COOKIES`

Less common load_flags:

 - `net::LOAD_BYPASS_CACHE`
 - `net::LOAD_DISABLE_CACHE`
 - `net::LOAD_DO_NOT_SEND_AUTH_DATA`

Other possible load_flags are listed in `net/base/load_flags_list.h`. You should include `net/base/load_flags.h` for those flags.

***

_(optional)_ Set a referrer (this should also be a `GURL` object):

```
resource_request->referrer = referrer_url;
```

***

_(optional)_ Set additional HTTP headers

`resource_request->headers` is a `net::HttpRequestHeaders` object that you can use to set additional HTTP headers. A common one is the `Authorization` header.

```
resource_request->headers.SetHeader(
      net::HttpRequestHeaders::kAuthorization,
      base::StringPrintf("Bearer %s", access_token_.c_str()));
```

Other possible resource request fields are listed in `services/network/public/cpp/resource_request.h`

## Creating The `TrafficAnnotationTag`

Include `network_traffic_annotation.h` at the top of your `.cc` file:

```
#include "net/traffic_annotation/network_traffic_annotation.h"
```

Create a `net::NetworkTrafficAnnotationTag` object by calling `net::DefineNetworkTrafficAnnotation()`. See [Network Traffic Annotations](https://chromium.googlesource.com/chromium/src/+/master/docs/network_traffic_annotations.md) for further documentation and examples of good network traffic annotation tags.

## Creating The `SimpleURLLoader`

Include `simple_url_loader.h` at the top of your `.cc` file:

```
#include "services/network/public/cpp/simple_url_loader.h"
```

Create your `SimpleURLLoader` object and pass it your `resource_request` and `traffic_annotation` objects.

**Important**: you need to consider your `SimpleURLLoader` object lifetime! If you create it as a local variable within a function, it will be deleted as soon as the function returns. As soon as the object is deleted, the network request will be canceled! All you will see is that your callback function is never called, and it will not be obvious what went wrong. You almost certainly want it as a private class variable, so it lives until your class is destroyed:

```
class MyClass {
...
private:
 std::unique_ptr<SimpleURLLoader> simple_url_loader_;
}
```

You can still create it on demand, just before calling it:

```
simple_url_loader_ = network::SimpleURLLoader::Create(
  std::move(resource_request), traffic_annotation);
```

***

_(optional)_ Set retry options

Before calling a download method, you can set retry options in case of network failure. It takes the number of times you automatically retry the network request, and a set of flags (`int` values OR'd together with `|`) to control what kind of network failure will trigger an automatic retry.

```
const kMyMaxRetries = 3;

...

simple_url_loader_->SetRetryOptions(kMyMaxRetries,
        network::SimpleURLLoader::RetryMode::RETRY_ON_5XX |
        network::SimpleURLLoader::RetryMode::RETRY_ON_NETWORK_CHANGE);
```

## Deciding on a `URLLoaderFactory`

A `URLLoaderFactory` is the low-level interface to the underlying Mojo classes that send and receive bytes over the wire. All the download methods of `SimpleURLLoader` take a `URLLoaderFactory` as a parameter. However, you probably don't want to pass around a `URLLoaderFactory` pointer, because their guaranteed lifetime is very short. If the network stack crashes and is reconstructed, all `URLLoaderFactory` pointers are invalidated. This happens more often than you might think.

In particular, **do not** pass a `URLLoaderFactory` pointer into your constructor, save it to a private variable, then assume you can use that pointer in another method later. This is unsafe and -- trust me when I tell you this -- very difficult to debug.

Instead, pass a `SharedURLLoaderFactory` into your constructor and save that. `SharedURLLoaderFactory` objects with live forever (or at least longer than your class), and it is safe to store a `SharedURLLoaderFactory` pointer when your class is constructed and reference it later.

Include `shared_url_loader_factory.h` at the top of your `.cc` file:

```
#include "services/network/public/cpp/shared_url_loader_factory.h"
```

Now, where do you get a `SharedURLLoaderFactory`? If you have a `Profile` or `BrowserContext`, use the one from the profile's default storage partition. If not, use the system-wide one from the system network context manager.

**Important**: you need to know whether your `SimpleURLLoader` could ever be used in a private window. If the answer is "yes," you MUST use the profile-specific `SharedURLLoaderFactory`, because it is proxied through Tor if the user has enabled that option. The system-wide `SharedURLLoaderFactory` is NEVER PROXIED, so using it for any reason in a private window could cause a privacy leak.

For the profile-specific `SharedURLLoaderFactory`, include these headers in the `.cc` file of the class that creates your class:

```
#include "content/public/browser/browser_context.h"
#include "content/public/browser/browser_thread.h"
#include "content/public/browser/storage_partition.h"
```

and call `content::BrowserContext::GetDefaultStoragePartition(profile())->GetURLLoaderFactoryForBrowserProcess()` to get a `SharedURLLoaderFactory` to pass into your class.

For the system-wide `SharedURLLoaderFactory`, just include this header instead (in the calling class):

```
#include "chrome/browser/net/system_network_context_manager.h"
```

Then call `g_browser_process->system_network_context_manager()->GetSharedURLLoaderFactory()`

Either way, what you have is a `scoped_refptr<network::SharedURLLoaderFactory>`, which is safe to store for the lifetime of your class.

Tip: use forward declarations in your class header file to reduce dependencies.

```
namespace network {
class SharedURLLoaderFactory;
class SimpleURLLoader;
}  // namespace network

class MyClass {
public:
  MyClass(scoped_refptr<network::SharedURLLoaderFactory> url_loader_factory);

private:
  scoped_refptr<network::SharedURLLoaderFactory> url_loader_factory_;
  std::unique_ptr<network::SimpleURLLoader> simple_url_loader_;
}
```

Pass the `SharedURLLoaderFactory` into your constructor and use `std::move` to store it:

```
class MyClass::MyClass(
  scoped_refptr<network::SharedURLLoaderFactory> url_loader_factory)
  : url_loader_factory_(std::move(url_loader_factory)) {
}
```

Then, when it's time to actually call `SimpleURLLoader`, use `url_loader_factory_.get()` to get a fresh `URLLoaderFactory` object to pass to the download method of your choice.

## Your Callback Function

When `SimpleURLLoader` is done, it will call the callback function that you specified when you called the download method. In the case of the `DownloadToString*` methods, your callback function looks like this:

```
void MyClass:OnSimpleLoaderComplete(std::unique_ptr<std::string> response_body) {
}
```

`response_body` is the body of the HTTP response, normalized into an `std::string` (i.e. not bytes). If there was any sort of error, `response_body` will be `null`. Always null-check `response_body`.  If you need a `response_body` for unsuccessful HTTP status codes, you can use `simple_url_loader_->SetAllowHttpErrorResults(true);`.

If there were any HTTP headers included in the response, the `headers` field on the original `simple_url_loader_` object will contain the response headers. The `headers` field can be null. Always null-check `simple_url_loader_->headers`.

You may be tempted to check the HTTP status code in your callback function. Consider whether this is really necessary. `!!response_body` is often sufficient to determine whether the request succeeded or failed. In case of failure, the `NetError` is often sufficient. (It's accessible inside your callback as `simple_url_loader_->NetError()`.)

However, if you need the actual HTTP status code, here is how to get it safely:

```
int response_code = -1;
if (simple_url_loader_->ResponseInfo() &&
  simple_url_loader_->ResponseInfo()->headers) {
  response_code =
    simple_url_loader_->ResponseInfo()->headers->response_code();
}
```

If you need more advanced callbacks, such as download progress or redirect notifications, see `simple_url_loader.h` for how to set up additional callbacks.

## Putting It All Together

This code from `components/ntp_tiles/popular_sites_impl.cc` downloads a file from `pending_url_`, without sending or saving cookies, and will automatically retry once if the network changes during the attempt:

```
  auto resource_request = std::make_unique<network::ResourceRequest>();
  resource_request->url = pending_url_;
  resource_request->load_flags =
      net::LOAD_DO_NOT_SEND_COOKIES | net::LOAD_DO_NOT_SAVE_COOKIES;
  simple_url_loader_ = network::SimpleURLLoader::Create(
      std::move(resource_request), traffic_annotation);
  simple_url_loader_->SetRetryOptions(
      1, network::SimpleURLLoader::RETRY_ON_NETWORK_CHANGE);
  simple_url_loader_->DownloadToStringOfUnboundedSizeUntilCrashAndDie(
      url_loader_factory_.get(),
      base::BindOnce(&PopularSitesImpl::OnSimpleLoaderComplete,
                     base::Unretained(this)));
```

This code from `browser/chromeos/policy/upload_job_impl.cc` uploads multi-part MIME data to `upload_url_`, with an `Authorization` header, and checks the HTTP headers of the response to see if it was successful:

```
  std::string content_type = kUploadContentType;
  content_type.append("; boundary=");
  content_type.append(*mime_boundary_.get());

  auto resource_request = std::make_unique<network::ResourceRequest>();
  resource_request->method = "POST";
  resource_request->url = upload_url_;
  resource_request->headers.SetHeader(
      net::HttpRequestHeaders::kAuthorization,
      base::StringPrintf(kAuthorizationHeaderFormat, access_token.c_str()));

  url_loader_ = network::SimpleURLLoader::Create(std::move(resource_request),
                                                 traffic_annotation_);
  url_loader_->AttachStringForUpload(*post_data_, content_type);
  url_loader_->DownloadHeadersOnly(
      url_loader_factory_.get(),
      base::BindOnce(&UploadJobImpl::OnURLLoadComplete,
                     base::Unretained(this)));
```

You can find many other examples in the Chromium code by searching for [`network::SimpleURLLoader::Create`](https://cs.chromium.org/search/?q=network::SimpleURLLoader::Create&type=cs).