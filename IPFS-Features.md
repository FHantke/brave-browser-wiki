## List of IPFS features

- Support of ipfs/ipns urls via gateway and localnode
- Public gateway customization
- Automatic redirection IPFS resources to IPFS gateway
- Automatic redirect DNSLink to an IPFS version of a website when possible
- IPFS address bar badge for pages with x-ipfs-header and dnslink TXT record for 5xx error code
- IPFS protocol menu in addressbar
- IPFS protocol system handler
- Interstitial page for Ask mode allows to install node or select public gateway (Android: public gateway only)

## Local-node only features
- Updating local node to new version
- Cache size configuration
- Peers list modification
- Diagnostic page (brave://ipfs)
  * Install/Start/Stop localnode
  * Show diagnostic information (Node Info, Repo Stats, Addresses, Connected Peers)
  * Perform a garbage collection sweep
  * Link to my node webui
  * Link to connected peers details
  
- IPNS keys management:
  * importing keys
  * pinning content with IPNS key
  * add/remove/rotate keys

- Import:
  * importing a page via IPFS
  * importing linked content
  * importing selected audio
  * importing selected image
  * importing selected text
  * importing selected video
  * sharing a local file using IPFS
  * sharing a local folder using IPFS

### Import
- [ ] Verify you can context-click on `wikipedia.org`, choose `Import to IPFS > This page` and see a `www.wikipedia.org` folder with assets, in the `Files` pane of the IPFS dashboard.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the resource.
- [ ] Verify you can context-click on the player UI for `https://dl.espressif.com/dl/audio/ff-16b-2c-44100hz.ogg`, choose `Import to IPFS > Selected audio`, and play the audio file found in the imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the audio file.
- [ ] Verify you can context-click on the Google logo at `www.google.com`, choose `Import to IPFS > Selected image`, and see the Google logo in its imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and it loads the image.
- [ ] Verify you can context-click on any selected text, choose `Import Selected Text to IPFS`, and... [TBD]
- [ ] Verify you can context-click on `https://upload.wikimedia.org/wikipedia/commons/c/c0/Big_Buck_Bunny_4K.webm`, choose `Import to IPFS > Selected video`, and play the video, found in its imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the video file.
- [ ] Verify you can choose `IPFS > Share Local File Using IPFS` to share a file of your choosing.  Confirm you can view/play the file from its imported folder.  Confirm a shareable, loadable `dweb.link` is copied to your clipboard, and that it loads the file.