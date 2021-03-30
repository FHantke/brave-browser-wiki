# chrome.ipfs

Allows a user to get information about the local IPFS node.

## Permissions

Requires `ipfs` permission

## Functions 

### chrome.ipfs.resolveIPFSURI(uri, callback(gateway_url))

- Parameters:
  - `uri`: An IPFS uri of the form `ipfs://[cid]`
  - `callback`: Callback called with the gateway URL of the passed in URI

### chrome.ipfs.getIPFSEnabled(callback(enabled))

- Parameters:
  - `callback`: Callback called with a boolean indicating if IPFS is enabled

IPFS Is enabled when it is not in a private Window, not disabled by admin policy, and enabled via chrome://flags (which is the default).
This does not indicate if the resolution type is set to a certain value.

### chrome.ipfs.getResolveMethodType(callback(type))

- Parameters:
  - `callback`: Callback called with a string `"ask"|"gateway"|"local"|"disabled"


## Availability

Available in Brave 1.24.x and later