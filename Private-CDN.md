The [Brave Private CDN](https://brave.com/brave-private-cdn/) is a way to
serve static files in a way that no single organization is able to see both
the client IP address and the nature of the request or response.

# When NOT to use the Private CDN

If your service **accepts data from users** and needs to strip the IP addresses, then the Private CDN is **not** for you. Use a TCP load-balancer instead. (See some [internal docs](https://github.com/brave/devops/wiki/Spectrum-configuration) about our typical config.)

If the content you're serving is the **same for all users** then the nature of the request/response doesn't need to be hidden and so all you need to do is strip the IP address on a regular CDN.

# How to use the Private CDN

Here's a typical workflow:

1. BraveFoo backend service receives `img0001.jpg` and `img0002.jpg`.
2. Backend pads the images (for example, using the [reference padding implementation](https://github.com/brave/simplepadding)).
3. Backend uploads `img0001.jpg.pad` and `img0002.jpg.pad` to the `pcdn` S3 bucket under `/bravefoo/`.
4. Client fetches `img0001.jpg.pad` and `img0002.jpg.pad` from `pcdn.brave.com`, omitting any identifying headers.
5. Client unpads the images (typically in memory using the brave-core [`RemovePadding()` function](https://github.com/brave/brave-core/blob/1cb5818aa0b70666c6aeea5ea9c06cc4e712171a/components/brave_private_cdn/private_cdn_helper.cc#L12)).
6. Client displays the images (typically as a `data:` URI).

Each service using the Private CDN will get its own set of AWS credentials which
only allow writing to the S3 keys which beging with a specific prefix
(`/bravefoo/` in the example above). File a [devops issue](https://github.com/brave/devops/issues/new/choose) to get started.

# Choosing the padding length

The length of the file that's passed to `simplepadding` is the final length
after padding.

This is the tricky part to get right, otherwise the padding is ineffective.

There are two basic ideas:

1. You want to create a **small number of sizes** that are available and pick
   the closest one for each image, based on the original size.
2. The **distribution of the sizes must be the same** across the various
   categories. That way, if someone were to observe all of your
   requests, they wouldn't be able to determine which categories you're
   interested in.

Brave News took the easiest approach: there is only one size (250kb) and all
images are padded to that size.

- The advantage is that you don't have to do any work to check that #2 is satisfied.
- The downside is that files are larger than they have to be.

# Choosing the file name

In addition to making the responses look identical (via padding), it's
important to make the **requests** look identical.

The way to do this is to make sure that the files stored in the S3 bucket
have names of a **fixed size**.

In the above example, we are fetching `img0001.jpg.pad` and not
`img1.jpg.pad` because that would have a different size then
`img10.jpg.pad`.

One way to achieve this is what Brave News did: simply make the filename be
the hash of the file contents. As long as you use the same hash function
throughout, then the filenames are guaranteed to have the same size.

Furthermore, this strategy makes it possible for the client to cache these
files forever since the filename will change should the content ever change.

# Request headers to omit

Clients which fetch images from the Private CDN should suppress the
following request headers as much as possible:

- `Accept-Language`
- `Cookie`
- `DNT`
- `Referer`
- `User-Agent`

Note that these headers are also filtered out on the CDN side, but the CDN
configuration cannot be examined by end users and so doing it on the client
is best.

# Development and staging URLs

The production endpoint is `pcdn.brave.com`, but there are also the following endpoints for staging/testing:

- Staging: `pcdn.bravesoftware.com`
- Dev: `pcdn.brave.software`