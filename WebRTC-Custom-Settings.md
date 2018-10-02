One of Brave's advanced security features is the ability to set a custom policy for how [WebRTC](https://en.wikipedia.org/wiki/WebRTC) handles your IP address. You can change the setting in `chrome://settings/privacy`. Most users will not need to do this.

## Policies

* Default: This is the default behavior in Brave when [Fingerprinting Protection](https://github.com/brave/browser-laptop/wiki/Fingerprinting-Protection-Mode) is off unless you are in a Tor tab. WebRTC has the right to enumerate all interfaces and bind them to discover public interfaces.
* Default Public And Private Interfaces:  WebRTC should only use the default route used by http. This also exposes the associated default private address. Default route is the route chosen by the OS on a multi-homed endpoint.
* Default Public Interface Only: WebRTC should only use the default route used by http. This doesn't expose any local addresses.
* Disable Non-Proxied UDP: WebRTC should only use TCP to contact peers or servers unless the proxy server supports UDP. This doesn't expose any local addresses either. This is the default when Fingerprinting Protection is on and when you are using a Tor tab.

## Learn More

See https://cs.chromium.org/chromium/src/content/public/common/webrtc_ip_handling_policy.h for where these policies are defined in the Chromium source code.