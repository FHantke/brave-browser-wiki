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

***
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

### Installation


### go-updater node update
- [ ] Verify going to `brave://ipfs` and clicking on `Install and start` installs and shows `go-ipfs/0.7.0` (or latest released), via the `Version:` section under `Node info`.
- [ ] Verify, using the above profile, that restarting Brave with `--use-dev-goupdater-url`, and clicking on `Restart` via `brave://ipfs` downloads and installs the latest development candidate, which at the time of this writing is `0.9.0-rc1` (we'll pick up the latest, so YMMV)


### Automatic redirects

#### IPFS
#### DNSLink
- [ ] Verify that loading `https://dweb.link/ipfs/QmT5NvUtoM5nWFfrQdVrFtvGfKFmG7AHE8P34isapyhCxX/wiki/Mars.html` redirects you seamlessly to `https://bafybeicgmdpvw4duutrmdxl4a7gc52sxyuk7nz5gby77afwdteh3jc5bqa.ipfs.dweb.link/wiki/Mars.html`, and there's an `Open using IPFS` badge/button in the URL bar.  Confirm that clicking `Open using IPFS` goes to `ipfs://bafybeicgmdpvw4duutrmdxl4a7gc52sxyuk7nz5gby77afwdteh3jc5bqa/wiki/Mars.html`.

### DNSLink

### IPFS Companion
- [ ] Verify that toggling `IPFS Companion` to `On` via `brave://settings/ipfs` prompts you to install the extension.  After clicking `Add extension`, confirm you get a notification that IPFS Companion was added to Brave, and are then taken to the `Set your IPFS preference` interstitial page.
- [ ] Verify that clicking on the puzzle-piece icon on the browser toolbar, then `IPFS Companion`, will load a popup.  Click on the gears (settings) icon and confirm it loads the `Companion Preferences` page.

### IPFS URLs
- [ ] Ensure each of the following IPFS URLs load over both `Gateway` and `Local node` modes:
* `ipfs://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq/wiki/Vincent_van_Gogh.html`
* `ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi/`
* `ipfs://QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Tokyo_National_Museum.html`
* `ipfs:QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme`

### IPNS URLs
- [ ] Ensure each of the following IPNS URLs load over both `Gateway` and `Local node` modes:
* `ipns://brantly.eth`
* `ipns://en.wikipedia-on-ipfs.org`
* `ipns://libp2p.io/`
* `ipns://en.wikipedia-on-ipfs.org/wiki/Tokyo_National_Museum.html`
* `ipns://browsers.today`
* `ipns://ipfs.io`

### Gateway
- [ ] Verify, on a new profile, you can load `brave://settings/ipfs`, click on the `Change` button for `IPFS public gateway address`, enter `https://cloudflare-ipfs.com/` and are presented with an interstitial page after loading `ipns://brantly.eth`.  Click `Use a public gateway` and confirm you're taken to `https://cloudflare-ipfs.com/ipns/brantly.eth/#/`.  (Alternatively, use one from `https://ipfs.github.io/public-gateway-checker/`.)
- [ ] Verify, on a new profile, you can load `https://wikipedia-on-ipfs.org`, switch `Method to resolve IPFS resources` to either `Gateway` or `Local node` in `brave://settings/ipfs`, and then see an `Open using IPFS` badge/icon in the URL bar.
- [ ] Verify clicking on `Open using IPFS` on `https://blog.ipfs.io/24-uncensorable-wikipedia` loads `ipfs://bafybeiaieqdmhtnehaau7kqoj2lmdfqc7juk34cjyb7dxr35vahp22bquu/24-uncensorable-wikipedia/`


### Diagnostic page
- [ ] Verify loading `brave://ipfs` redirects to `brave://ipfs-internals`
- [ ] Verify, on a clean profile, visiting `brave://ipfs` will present you with an `Install and start` button, which will install and start an IPFS local node.  Confirm you see `Node is running` under `IPFS node status`, `Stop`, `Restart`, and `My Node` buttons, and a dynamically updating `Connected peers:` count.
- [ ] Verify that clicking `Stop` resets all statistics, paths, and config information.
- [ ] Verify that clicking `Start` populates all statistics, paths, and config information, and you see `Stop`, `Restart`, and `My Node` buttons, along with a dynamically updating `Connected peers:` count. 
- [ ] Verify that clicking `Restart` resets and repopulates all stats, paths, and config information, and you see `Stop`, `Restart`, and `My Node` button, along with a dynamically updating `Connected peers:` count.
- [ ] Verify that clicking on `(details)` to the right of `Connected peers:` takes you to the `PEERS` pane of `127.0.0.1:45002/ipfs/bafy..../#/` and you see a global map with a dynamically updating peer count and listing, below.
- [ ] Verify that clicking on `Perform a garbage collection sweep` resets and repopulates the `Objects:` and `Size:` stats beneath `Repo Stats`.
- [ ] Verify that clicking `My Node` takes you to the `Status` pane of the IPFS dashboard, with a URL similar to `127.0.0.1:45002/ipfs/bafy..../#/`, where you see `Connected to IPFS`, MB count of files shared, and dynamically updating peers count, as well as your `PEER ID` and `AGENT`.


### Import
- [ ] Verify you can context-click on `wikipedia.org`, choose `Import to IPFS > This page` and see a `www.wikipedia.org` folder with assets, in the `Files` pane of the IPFS dashboard.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the resource.
- [ ] Verify you can context-click on the player UI for `https://dl.espressif.com/dl/audio/ff-16b-2c-44100hz.ogg`, choose `Import to IPFS > Selected audio`, and play the audio file found in the imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the audio file.
- [ ] Verify you can context-click on the Google logo at `www.google.com`, choose `Import to IPFS > Selected image`, and see the Google logo in its imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and it loads the image.
- [ ] Verify you can context-click on any selected text, choose `Import Selected Text to IPFS`, and... [TBD]
- [ ] Verify you can context-click on `https://upload.wikimedia.org/wikipedia/commons/c/c0/Big_Buck_Bunny_4K.webm`, choose `Import to IPFS > Selected video`, and play the video, found in its imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the video file.
- [ ] Verify you can choose `IPFS > Share Local File Using IPFS` to share a file of your choosing.  Confirm you can view/play the file from its imported folder.  Confirm a shareable, loadable `dweb.link` link is copied to your clipboard, and that it loads the file.
- [ ] Verify you can choose `IPFS > Share Local Folder Using IPFS` to share a folder of your choosing.  Confirm the file contents and sizes are identical to the originals.  Confirm a shareable, loadable `'dweb.link` link is copied to your clipboard, and that it loads the file.

### Protocol system handler/OS integration
- [ ] Verify (`Windows`) that pressing `Win+R`, typing `ipfs://bafkreigcnxudvpojjfwncmauociy5q46zsq46oe66cxbyzie3imabuoege`, and pressing `Enter` opens Brave and loads an HTML page with the word `PASS`.
- [ ] Verify (`macOS`): opening Terminal, and typing `open ipfs://bafkreigcnxudvpojjfwncmauociy5q46zsq46oe66cxbyzie3imabuoege`, and pressing `Enter` opens Brave and loads an HTML page with the word `PASS`.
- [ ] Verify (`Linux`) that opening a shell and typing `xdg-open ipfs://bafkreigcnxudvpojjfwncmauociy5q46zsq46oe66cxbyzie3imabuoege` opens Brave and loads an HTML page with the word `PASS`.

### Peers
## adding
## removing
### Key management
### Pinning content using keys