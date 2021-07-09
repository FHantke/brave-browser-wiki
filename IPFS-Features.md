# IPFS on Desktop


## Installation & Setup

   ### `go-updater node update`
  
   - [ ] Verify going to `brave://ipfs` and clicking on `Install and start` installs and shows the latest `go-ipfs` release (`https://github.com/ipfs/go-ipfs/blob/master/CHANGELOG.md`), via the `Version:` section under `Node info`.
   - [ ] Verify, using the above profile, that restarting Brave with `--use-dev-goupdater-url`, and clicking on `Restart` via `brave://ipfs` downloads and installs the latest in-development (release) candidate.
   - [ ] Verify, after installing the latest update, that you still have the in-development version, upon restart, running without `--use-dev-goupdater-url`.  (Doing so will cause problems with installing IPFS Companion, later.)

   ### `Config`

  - [ ] Verify changing `Maximum IPFS cache size (GB)` on the `brave://settings/ipfs` page (set `Method to resolve IPFS resources` to `Local node` on `brave://settings/ipfs`), the new value is reflected on the diagnostic page (`brave://ipfs`) in the `Repo Stats -> Size` section.

## Diagnostic page (`brave://ipfs`)

- [ ] Verify loading `brave://ipfs` redirects to `brave://ipfs-internals`.
- [ ] Verify, on a clean profile, visiting `brave://ipfs` will present you with an `Install and start` button, which will install and start an IPFS local node.  Confirm you see `Node is running` under `IPFS node status`, `Stop`, `Restart`, and `My Node` buttons, and a dynamically updating `Connected peers:` count.
- [ ] Verify that clicking `Stop` temporarily clears all statistics, paths, and config information.
- [ ] Verify that clicking `Start` populates all statistics, paths, and config information, and you see `Stop`, `Restart`, and `My Node` buttons, along with a dynamically updating `Connected peers:` count. 
- [ ] Verify that clicking `Restart` clears and then repopulates all stats, paths, and config information, and you see `Stop`, `Restart`, and `My Node` buttons, along with a dynamically updating `Connected peers:` count.
- [ ] Verify that clicking on `(details)` to the right of `Connected peers:` takes you to the `PEERS` pane of `127.0.0.1:45002/ipfs/bafy..../#/` and you see a global map with a dynamically updating peer count and listing, below.
- [ ] Verify that clicking on `Perform a garbage collection sweep` resets and repopulates the `Objects:` and `Size:` stats beneath `Repo Stats`.
- [ ] Verify that clicking `My Node` takes you to the `Status` pane of the IPFS dashboard, with a URL similar to `127.0.0.1:45002/ipfs/bafy..../#/`, where you see `Connected to IPFS`, MB count of files shared, and dynamically updating peers count, as well as your `PEER ID` and `AGENT`.

## Import and Sharing

- [ ] Prerequisites: local node launched and local gateway configured.  On a new profile, loading `ipns://brantly.eth` and clicking `Use a local node` on the interstitial page will set you up.

   ### `Importing a page via IPFS`
    
   - [ ] Load `wikipedia.org`. Context-click the page and select `Import to IPFS > This page`. Make sure the content is downloaded and imported, the import folder is opened when import completed successfully, the page content can be downloaded. Verify the shareable link is copied to the clipboard and opens when pasting it.

   ### `Importing linked content`

   - [ ] Load `https://search.brave.com/search?q=hiddenmickeys&source=desktop`.  Context-click the first link there and select `Import to IPFS > Linked content`.  Make sure the content is downloaded and imported, the import folder is opened when import completed successfully, and the file can be downloaded. Verify the shareable link is copied to the clipboard and opens when pasting it.

   ### `Importing selected audio`

   - [ ] Load `https://samplelib.com/sample-mp3.html`.  Context-click any audio file and select `Import to IPFS > Selected audio`. Make sure the audio is downloaded and imported, the import folder is opened when import completed successfully, and the file can be downloaded. Verify the shareable link is copied to the clipboard and opens when pasting it.

   ### `Importing selected image`
   
   - [ ] Load `google.com`. Select the logo and choose `Import to IPFS > Selected image` and import it. Make sure the image is downloaded and imported, the import folder is opened when import completed successfully, the file can be downloaded. Verify the shareable link is copied to the clipboard, and opens, when pasting it.

      Verify for the following filetypes:
       - [ ] .apng
       - [ ] .gif
       - [ ] .jpg/.jpeg
       - [ ] .svg
       - [ ] .webp

   ### `Importing selected text`

   - [ ] Load `https://lipsum.com/` and make a text selection.  Context-click and choose `Import Selected Text to IPFS`. Make sure the text is wrapped into a file with id like `file_1` and the imported text is available inside the file. Verify the shareable link is copied to the clipboard and opens when pasting it.

   ### `Importing selected video`

   - Load `https://upload.wikimedia.org/wikipedia/commons/c/c0/Big_Buck_Bunny_4K.webm`. Play the video, context-click on it, and choose `Import to IPFS > Selected video`. Make sure the video is downloaded and imported, the import folder is opened when import completed successfully, the file can be downloaded. Verify the shareable link is copied to the clipboard and opens when pasting it.  For now, only the above Big Bucky test file needs to play inline, until I can figure out support for native/non-native file types.  For the other types, downloading from the IPFS webui (My Node) and ensuring they still play in a media player, is enough.

      Verify for the following filetypes:
       - [ ] .avi
       - [ ] .mov
       - [ ] .mp4
       - [ ] .ogg
       - [ ] .webm

   ### `Sharing a local file using IPFS (without keys)`

   - Verify the IPFS item available in the main app menu. Go to `IPFS -> Share Local File Using IPFS` select and import any local file. Make sure the file is imported, the import folder is opened when import completed successfully, the file can be downloaded and the downloaded one is same as original. The shareable link is copied to the clipboard.  Share the following filetypes:

       - [ ] .avi
       - [ ] .txt
       - [ ] .json
       - [ ] .mpeg
       - [ ] .mp3
       - [ ] .mp4
       - [ ] .ogg
       - [ ] .wav
       - [ ] .webm

   ### `Sharing a local folder using IPFS (without keys)`

   - [ ] Go to `IPFS -> share Local Folder Using IPFS`, and select and import any local folder. Make sure the whole folder is imported, the import folder is opened when import completed successfully, files can be downloaded, and the downloaded one is same as original. Verify the shareable link is copied to the clipboard, and loads.

   ### `Automatic redirects to IPFS`

   - [ ] Via `brave://settings/ipfs`, set `Redirect IPFS resources to the configured IPFS gateway` to `On`.  Load `https://en.wikipedia-on-ipfs.org/` and confirm it redirects to `http://en.wikipedia-on-ipfs.org.ipns.localhost:48081/wiki/`.
   - [ ] Via `brave://settings/ipfs`, set `Automatically redirect to IPFS pages via DNSLink when possible` to `On`.  Load `https://en.wikipedia-on-ipfs.org/wiki/` and confirm it automatically redirects to `ipns://en.wikipedia-on-ipfs.org/wiki/`.
   - [ ] Verify, when loading an IPFS/IPNS resources on a new profile (such as `ipns://brantly.eth`), that the interstitial page (`Ask` mode in `brave://settings/ipfs`) gives you two choices: 1) to install a local node (`Use a local node`) or 2) `Use a public gateway` (Android: public gateway only).
   - [ ] Load `ipns://brantly.eth` while using `Local node` for the resolver, and confirm there's a clickable info badge to the left of the URL, which says `This content was loaded over the IPFS protocol.`


## `DNSLink`

- [ ] Verify that loading `https://dweb.link/ipfs/QmT5NvUtoM5nWFfrQdVrFtvGfKFmG7AHE8P34isapyhCxX/wiki/Mars.html` redirects you seamlessly to `https://bafybeicgmdpvw4duutrmdxl4a7gc52sxyuk7nz5gby77afwdteh3jc5bqa.ipfs.dweb.link/wiki/Mars.html`, and there's an `Open using IPFS` badge/button in the URL bar.  Confirm that clicking `Open using IPFS` goes to `ipfs://bafybeicgmdpvw4duutrmdxl4a7gc52sxyuk7nz5gby77afwdteh3jc5bqa/wiki/Mars.html`.
- [ ] Verify that loading `https://ipfs.io/ipns/libp2p.io/` shows an `Open using IPFS` button in the URL bar, and clicking it redirects to `ipns://libp2p.io/`.  Confirm it resolves and loads.

## `IPFS Companion`

- [ ] Verify that toggling `IPFS Companion` to `On` via `brave://settings/ipfs` prompts you to install the extension.  After clicking `Add extension`, confirm you get a notification that IPFS Companion was added to Brave, and are then taken to the `Set your IPFS preference` interstitial page.
- [ ] Verify that clicking on the puzzle-piece icon on the browser toolbar, then `IPFS Companion`, will load a popup.  Click on the gears (settings) icon and confirm it loads the `Companion Preferences` page.

## `IPFS URLs`

- [ ] Ensure each of the following IPFS URLs load over both `Gateway` and `Local node` modes:
    - [ ] `ipfs://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq/wiki/Vincent_van_Gogh.html`
    - [ ] `ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi/`
    - [ ] `ipfs://QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Tokyo_National_Museum.html`
    - [ ] `ipfs:QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme`

### `IPNS URLs`

- [ ] Ensure each of the following IPNS URLs load over both `Gateway` and `Local node` modes:
   - [ ] `ipns://brantly.eth`
   - [ ] `ipns://en.wikipedia-on-ipfs.org`
   - [ ] `ipns://libp2p.io/`
   - [ ] `ipns://en.wikipedia-on-ipfs.org/wiki/Tokyo`
   - [ ] `ipns://ipfs.io`

## `Gateway`

- [ ] Verify, on a new profile, you can load `brave://settings/ipfs`, click on the `Change` button for `IPFS public gateway address`, enter `https://cloudflare-ipfs.com/` and are presented with an interstitial page after loading `ipns://brantly.eth`.  Click `Use a public gateway` and confirm you're taken to `https://cloudflare-ipfs.com/ipns/brantly.eth/#/`.  (Alternatively, use one from `https://ipfs.github.io/public-gateway-checker/`.)
- [ ] Verify, on a new profile, you can load `https://wikipedia-on-ipfs.org`, switch `Method to resolve IPFS resources` to either `Gateway` or `Local node` in `brave://settings/ipfs`, and then see an `Open using IPFS` badge/icon in the URL bar.
- [ ] Verify clicking on `Open using IPFS` on `https://blog.ipfs.io/24-uncensorable-wikipedia` loads `ipfs://bafybeiaieqdmhtnehaau7kqoj2lmdfqc7juk34cjyb7dxr35vahp22bquu/24-uncensorable-wikipedia/`

### Protocol system handler/OS integration
- [ ] Verify (`Windows`) that pressing `Win+R`, typing `open ipfs://bafkreigcnxudvpojjfwncmauociy5q46zsq46oe66cxbyzie3imabuoege`, and pressing `Enter` opens Brave and loads an HTML page with the word `PASS`.
- [ ] Verify (`macOS`): opening Terminal, and typing `open ipfs://bafkreigcnxudvpojjfwncmauociy5q46zsq46oe66cxbyzie3imabuoege`, and pressing `Enter` opens Brave and loads an HTML page with the word `PASS`.
- [ ] Verify (`Linux`) that opening a shell and typing `xdg-open ipfs://bafkreigcnxudvpojjfwncmauociy5q46zsq46oe66cxbyzie3imabuoege` and pressing `Enter` opens Brave and loads an HTML page with the word `PASS`.

## `Peers`

- [ ] Prerequisites: local node launched and local gateway configured on two machines, locally networked (LAN, can be over Wi-Fi).

   ### `Adding` (see https://github.com/brave/brave-browser/issues/15567#issuecomment-867983572 for full setup steps)

   - [ ] Verify when you go to `brave://settings/ipfs/peers` and click on the `Add` button, it prompts you to enter a new peer-connection string. Confirm that entering an incorrect string yields `This name is not valid` upon clicking `Submit`. (Acceptable ones are only CIDs or something like `**/p2p/**` format.)
   - [ ] Verify if a peer is added and node is started, it proposes to restart node to apply changes.
   - [ ] Verify the local node is restarted by clicking `Restart` button; happen it shows error message and suggests to see more on diagnostic page.

   ### `Removing`

   - [ ] Verify a peer can be removed by clicking on Trash icon in the peer line.

## `IPNS Keys`

- [ ] Prerequisites: local node launched and local gateway configured. Go to `Settings -> IPFS`, there should be an available `Set up your IPNS keys` option, which opens `brave://settings/ipfs/keys`

   ### `Sharing a local file using an IPFS key`

   - [ ] Verify the IPFS item available in the main app menu. Go to IPFS -> Share Local File Using IPFS select and import any local file. Make sure the file is imported, the import folder is opened when import completed successfully, the file can be downloaded and the downloaded one is same as original. The shareable link is copied to the clipboard; verify you see your key before the `?filename=filename.ext` in the copied text, e.g. `https://dweb.link/ipns/k51qzi5uqu5dgxhiv8w8cdvmgdhbvy3t9gn4jwpwwro18fots0xtdabpcxxzwc?filename=Big_Buck_Bunny_4K.webm` (the key is `k51qzi5uqu5dgxhiv8w8cdvmgdhbvy3t9gn4jwpwwro18fots0xtdabpcxxzwc`).

   ### `Sharing a local folder using an IPFS key`

   - [ ] Verify the IPFS item available in the main app menu. Go to `IPFS -> share Local Folder Using IPFS`, and select and import any local folder. Make sure the whole folder is imported, the import folder is opened when import completed successfully, files can be downloaded and the downloaded one is same as original. The shareable link is copied to the clipboard; verify you see your key before the `?filename=Downloads` part, e.g. `https://dweb.link/ipns/k51qzi5uqu5djfh24zd6m4e8fm6d9rju48fyokc13achcfo4hz9fmioev0xln6?filename=Downloads` (`k51qzi5uqu5djfh24zd6m4e8fm6d9rju48fyokc13achcfo4hz9fmioev0xln6` is the key here).

   ### `Importing keys`

   - [ ] Verify adding a new key by clicking on the `Import` button and choosing an existing key file to import.
   - [ ] Verify imported key is available with entered name; verify entering `self` will yield `This name cannot be used`.
   - [ ] Verify you cannot import the same key twice.

   ### `Pinning content with IPNS key`

   - [ ] Verify keys are available in all import menus in order to pin content by selected key. the import link should contain the selected key.

   ### `Add/Remove/Rotate keys`

   - [ ] Verify when you click `Add`, it prompts for key name and generates a new key.
   - [ ] Verify clicking on `Add` and entering an existing key name shows a `This name cannot be used` error message.
   - [ ] Verify when you click on the Trash icon for a key, it removes the key.


***

# IPFS on Android

### `Public Gateway Setting`

- [ ] Verify `IPFS Gateway` option is available under `Brave Shields & Privacy`
- [ ] Verify the setting is enabled by default
- [ ] Verify disable/enable setting is retained between browser launch/restarts
- [ ] Verify setting state is retained during upgrade

### `IPFS/IPNS URI`

- [ ] Verify visiting `ipfs://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq/wiki/Vincent_van_Gogh.html` for the first time triggers IPFS interstitial page to select public gateway to load the URI
  - [ ] Verify selecting `Use a public gate` loads `https://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq.ipfs.dweb.link/wiki/Vincent_van_Gogh.html` via public gateway
- [ ] Verify visiting `ipns://brave.crypto` brings up IPFS interstitial page
   - [ ] Verify selecting `Use a public gate` loads `https://brave-crypto.ipns.dweb.link` via public gateway
- [ ] Verify when setting is disabled, loading an `ipfs://` URI or `ipns://` URI doesn't show any interstitial page 
- [ ] Verify `ipfs://` URI or `ipns://` URI doesn't load on a private tab even when the setting is enabled