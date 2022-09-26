One (of many) significant changes Brave makes to Chromium (compared to other Chromium based browsers, like Edge and Chrome) is to the lifetime and scope of permissions granted to websites through the [Permissions API](https://developer.mozilla.org/en-US/docs/Web/API/Permissions_API).  Sites use the Permissions API to request access to privacy-sensitive capabilities, such as geolocation or Web cams.

Brave broadly improves the privacy of the Permissions API in two ways.

The first way Brave improves the privacy of the Permissions API is to give users more control over the *scope* of permissions. In other Chromium based browsers, permissions are granted globally; if you give (for example) Google Maps access to your geolocation, Google Maps can access your geolocation both when you're visiting Google Maps directly (i.e., https://www.google.com/maps), or when Google Maps is embedded on another website.

Brave improves this situation by giving users more control. Instead of permissions decisions being global, Brave partitions permissions by site. In practice, this means you can give site.example one set of permissions when you're visiting the site directly, another set of permissions when site.example is embedded on cats.com, and a third set of permissions when site.example is embedded on dogs.com.

The second way Brave improves the privacy of the Permissions API is to give users more control over the *lifetime* of permissions. In other Chromium browsers, you can either give a site permission for a capability forever, or not at all. This causes users to give sites more access to more sensitive capabilities for longer than sites need them, and often for longer than users can keep track of.

Again, Brave improves this situation by giving users more control. In Brave, you can give sites a permission for a limited time (for example, for one day, or until you close the site), in addition to the "forever" or "deny" options available in Chrome.

More information on how Brave improves the privacy of the Permissions API can be found our [related blog post](https://brave.com/privacy-updates/8-grab-bag-2/#improved-web-permission-lifetimes).