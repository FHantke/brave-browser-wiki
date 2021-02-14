This page will serve as a guide through the backend architecture of the current iteration of widgets that exist on the new tab page. At time of writing, this includes Binance, Gemini, and Crypto.com. Throughout the guide, the Gemini widget will be used as a code reference due to the robustness of its features.

# Service Layer

## Factory
Each widget has its own service. This is where business logic for networking and preferences modification live. The factory for a new widget service should live under `browser/new_widget`. It's important to create methods to be able to access a widget service for a given profile from a static context:

```
// static
NewWidgetServiceFactory* NewWidgetServiceFactory::GetInstance() {
  return base::Singleton<NewWidgetServiceFactory>::get();
}

// static
NewWidgetService* NewWidgetServiceFactory::GetForProfile(Profile* profile) {
  if (profile->IsIncognitoProfile() ||
      profile->IsGuestSession()) {
    return nullptr;
  }
  return static_cast<NewWidgetService*>(
      GetInstance()->GetServiceForBrowserContext(profile, true));
}
```
[Code Reference](https://github.com/brave/brave-core/blob/master/browser/gemini/gemini_service_factory.cc)

## Service
The service file itself should live under `components/new_widget/browser`. It will need to implement the following classes:

```
namespace base {
class FilePath;
class SequencedTaskRunner;
}  // namespace base

namespace content {
class BrowserContext;
}  // namespace content

namespace network {
class SharedURLLoaderFactory;
class SimpleURLLoader;
}  // namespace network

class Profile;
```

### Networking
A network annotation should exist to describe the necessity of the network requests. It should reference the third party service and give instructions to disable the feature. This message of type `net::NetworkTrafficAnnotationTag` will be sent along with every request.

Using `network::SimpleURLLoader` - each service class should define a `Request` method. Params differ depending on the needs of the service, but in addition to request data, it should accept a callback typed as the following:

```
using URLRequestCallback =
    base::OnceCallback<void(const int, const std::string&,
                            const std::map<std::string, std::string>&)>;
```

The parameters correspond to the returned HTTP status, the response body and response headers. This is ultimately called when the URLLoader has completed its request.

Code references:
[[1]](https://github.com/brave/brave-core/blob/master/components/gemini/browser/gemini_service.cc#L437)
[[2]](https://github.com/brave/brave-core/blob/master/components/gemini/browser/gemini_service.cc#L488)

### Oauth
If the widget supports authentication via oauth, the service layer will need to handle both the flow and secure storage of credentials such as access and refresh tokens. As these widgets exist on a NTP, a custom protocol is defined in order to act as a bridge between the return of the access token from the third party service and the NTP, as we cannot redirect directly to it. Instructions for defining a custom scheme are [here](https://github.com/brave/brave-browser/wiki/Adding-a-protocol-scheme-to-Brave), and an example protocol handler can be found [here](https://github.com/brave/brave-core/blob/master/browser/gemini/gemini_protocol_handler.cc).

In compliance with Oauth2 standards and Brave requirements, the service should implement the following methods:

```
std::string GetOAuthClientUrl(); // Should return a constructed URL to initiate an oauth request
bool GetAccessToken(AccessTokenCallback callback); // Makes request to get access token
bool RefreshAccessToken(AccessTokenCallback callback); // Makes request for a new access token, using the stored refresh token
bool RevokeAccessToken(RevokeAccessTokenCallback callback); // Makes request to invalidate the session with the current access token.
```

Should the third party service support PKCE flow, there is a defined utility `ntp_widget_utils` that contains methods to validate crypto random code challenges, it can be found [here](https://github.com/brave/brave-core/blob/master/components/ntp_widget_utils/browser/ntp_widget_utils_oauth.cc). Usage:

```
std::string verifier = ntp_widget_utils::GetCryptoRandomString(false);
std::string challenge = ntp_widget_utils::GetCodeChallenge(verifier, false);
```

Credentials such as the refresh and access token should be stored in chromium prefs. `OSCrypt` is the encryption utility used on these values, and it is only available if the user has given Brave access to their keyring.

When storing a token, first encrypt the string:
```
std::string encrypted_access_token;
if (!OSCrypt::EncryptString(access_token, &encrypted_access_token)) {
  LOG(ERROR) << "Could not encrypt and save the access token";
}
```

Then, base64 encode the encrypted string:
```
std::string encoded_encrypted_access_token;
base::Base64Encode(encrypted_access_token, &encoded_encrypted_access_token);
```

Then store the result:
```
prefs->SetString(kNewWidgetAccessToken, encoded_encrypted_access_token);
```

When loading the tokens to use, just do the steps in reverse, using `OSCrypt::DecryptString` to decrypt.

### Response Parsing
Each widget should implement its own JSON parser utility for handling network responses. The parser methods should accept the response body as a string, as well as a pointer to the variable where the data is being extracted to, so it may modify it in place.

Usage within a request callback in the widget service
```
void NewWidgetService::OnGetStatus(
    StatusCallback callback,
    const int status, const std::string& body,
    const std::map<std::string, std::string>& headers) {
  std::string status;
  if (status >= 200 && status <= 299) {
    NewWidgetJSONParser::GetStatusFromJSON(body, &status);
  }
  std::move(callback).Run(status);
}
```

Parser method:
```
// static
// Response Format
// {
//   "status": "success",
// }
//
bool NewWidgetJSONParser::GetTokensFromJSON(
    const std::string& json, std::string *status) {
  base::JSONReader::ValueWithError value_with_error =
      base::JSONReader::ReadAndReturnValueWithError(
          json, base::JSONParserOptions::JSON_PARSE_RFC);
  base::Optional<base::Value>& records_v = value_with_error.value;

  if (!records_v) {
    return false;
  }

  const base::Value* status_val = records_v->FindKey("status");

  if (status_val && status_val->is_string()) {
    *status = status_val->GetString();
  }

  return true;
}
```
[Code Reference](https://github.com/brave/brave-core/blob/master/components/gemini/browser/gemini_json_parser.cc)

### Region Availability
As is the case with the current integrated crypto exchanges, there may be regions where the widget and all of its dependencies should be disabled for regulatory purposes. The service should expose a method `bool IsSupportedRegion()` that can be used when setting build vars.

A file of static regions should exist in `/components/new_widget/browser` to power this check. Depending on which makes sense, it should export a vector of strings that is either an allow list or deny list of regions. An example is [here](https://github.com/brave/brave-core/blob/master/components/gemini/browser/regions.h)

A utility exists to check the users install region against an allow or deny list [here](https://github.com/brave/brave-core/blob/master/components/ntp_widget_utils/browser/ntp_widget_utils_region.cc). It can be used as such:

```
bool NewWidgetService::IsSupportedRegion() {
  PrefService* prefs = user_prefs::UserPrefs::Get(context_);
  // If using an deny list
  // bool is_supported = ntp_widget_utils::IsRegionSupported(
  //    prefs, ::new_widget::unsupported_regions, false);
  // If using an allow list
  // bool is_supported = ntp_widget_utils::IsRegionSupported(
  //    prefs, ::new_widget::supported_regions, true);
  return is_supported;
}
```

### Testing
Widget services should be accompanied by browser tests for good coverage. The browser tests should ensure that everything works end to end, service call to parsed response. `net::test_server` can be used to mock HTTP response per endpoint being tested. Methods should be exposed in the service that allow for the setting of remote host and other credentials from within the test environment. For example, if a service relies on a client secret, you'd want to create:

```
void NewWidgetService::SetClientSecretForTest(const std::string& client_secret) {
  client_secret_ = client_secret;
}

void NewWidgetService::SetApiHostForTest(const std::string& api_host) {
  api_host_ = api_host;
}
```

Be sure to have the browser test class set as a friend of the service: `friend class NewWidgetBrowserTest;`

[Code Reference](https://github.com/brave/brave-core/blob/master/components/gemini/browser/gemini_service_browsertest.cc)

# API
In order for WebUI to interact with the backend service, an API under the `chrome` namespace will need to exposed to act as a bridge.

## Definitions
Two configurations will need to be defined describing the API methods under `common/extensions/api`, an example to create `chrome.newWidget`:

```
// new_widget.json
[
    {
      "namespace": "newWidget",
      "description": "Use the <code>chrome.newWidget</code> to interact with the Widget services.",
      "compiler_options": {
        "implemented_in": "brave/browser/extensions/api/new_widget_api.h"
      },
      "events": [
      ],
      "functions": [
        {
          "name": "getClientUrl",
          "type": "function",
          "description": "Fetches the Oauth Url for the widget",
          "parameters": [
            {
              "type": "function",
              "name": "callback",
              "parameters": [
                {
                  "name": "clientUrl",
                  "type": "string"
                }
              ]
            }
          ]
        }
      ],
      "types": [
      ],
      "properties": {
      }
    }
  ]
```

And to ensure it has access to the NTP:

```
new_widget_features.json

{
    "newWidget": {
      "channel": "stable",
      "contexts": ["webui"],
      "dependencies": [],
      "matches": [
        "chrome://newtab/*"
      ]
   }
}
```

To support the API within the Typescript env, declare the namespace in `components/definitions/chromel.d.ts`:

```
declare namespace chrome.newWidget {
  const getClientUrl: (callback: (clientUrl: string) => void) => {}
}
```

## Writing the API

Brave's chrome APIs are defined in `browser/extensions/api`. `new_widget_api.cc`:

```
namespace {
// Convenience method for getting the service context
NewWidgetService* GetNewWidgetService(content::BrowserContext* context) {
  return NewWidgetServiceFactory::GetInstance()
      ->GetForProfile(Profile::FromBrowserContext(context));
}

// All API endpoints should be guarded with a check using `brave::IsRegularProfile`.
// This makes the API unavailable from within Tor and private windows
bool IsNewWidgetAPIAvailable(content::BrowserContext* context) {
  return brave::IsRegularProfile(context);
}

}  // namespace

namespace extensions {
namespace api {

ExtensionFunction::ResponseAction
NewWidgetGetClientUrlFunction::Run() {
  if (!IsNewWidgetAPIAvailable(browser_context())) {
    return RespondNow(Error("Not available in Tor/incognito/guest profile"));
  }
  auto* service = GetNewWidgetService(browser_context());
  const std::string client_url = service->GetOAuthClientUrl();
  return RespondNow(OneArgument(base::Value(client_url)));
}

}  // namespace api
}  // namespace extensions
```

`new_widget_api.h`:

```
class Profile;

namespace extensions {
namespace api {

class NewWidgetGetClientUrlFunction :
    public ExtensionFunction {
 public:
  DECLARE_EXTENSION_FUNCTION("newWidget.getClientUrl", UNKNOWN)

 protected:
  ~NewWidgetGetClientUrlFunction() override {}
  ResponseAction Run() override;
};

}  // namespace api
}  // namespace extensions
```

Code references:
[[1]](https://github.com/brave/brave-core/blob/master/browser/extensions/api/gemini_api.cc)
[[2]](https://github.com/brave/brave-core/blob/master/browser/extensions/api/gemini_api.h)
