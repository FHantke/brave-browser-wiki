Brave uses the same User Agent request header as Chromium for web compatibility reasons. In testing, we found that some websites broke on encountering an uncommon User Agent. 

If you want to detect Brave, please use the `navigator.brave.isBrave()` JavaScript API.