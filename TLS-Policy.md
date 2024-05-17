Here are some details about how [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) or TLS (formerly SSL) works in Brave.

# Implementation

On all platforms except iOS, we currently use the upstream Chromium implementation unmodified.

On iOS, TLS connections are handled by the Operating System.

# Root Store

On each platform that we support, except for iOS, we use the same default root store as Chrome.

On iOS, we rely on the Apple root store.

# Root pinning and HSTS preloading

While we do enable root pinning enforcement and HSTS preloading, we do not currently use the list that ships with Chrome. Instead, we have our own lists: https://github.com/brave/brave-core/blob/master/chromium_src/net/tools/transport_security_state_generator/input_file_parsers.cc

You can confirm that it is working in your browser using this test domain: https://ssl-pinning.someblog.org/

# Certificate Transparency

On iOS, we rely on Apple's CT policy and the CT support in Webkit.

On desktop & Android, we follow Chrome's CT policy and started enforcement in 1.56 (https://github.com/brave/brave-core/pull/17944). SCT auditing is disabled.