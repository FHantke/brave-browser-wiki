Chrome extensions are generally available in Brave, at the user's risk. These are not reviewed by Brave and user should be cautious.

There are two ways that extensions are blocked in Brave: via [Google Safe Browsing](https://safebrowsing.google.com/), and via a Brave-controlled mechanism.

# Safe Browsing list

The Safe Browsing bad extension list comes from Google and is used to protect users in Brave (unless they have opted out of Safe Browsing protection via the appropriate setting in `brave://settings/security`).

In order to test that the extension blocklist portion of Safe Browsing is working in Brave, you'll need to create a special build of Brave and a local proxy like [mitmproxy](https://mitmproxy.org/).

Here are the full instructions:

1. Get a local [Brave checkout](https://github.com/brave/brave-browser/blob/master/README.md#clone-and-initialize-the-repo).
2. Apply [this patch](https://gist.github.com/fmarier/3cf69f2d6696d44919784b62d9613a06).
3. [Compile Brave](https://github.com/brave/brave-browser/blob/master/README.md#build-brave).
4. [Install](https://docs.mitmproxy.org/stable/overview-installation/) mitmproxy and [install its certificate authority](https://docs.mitmproxy.org/stable/concepts-certificates/) in Brave.
5. Start mitmproxy in a terminal:

       mitmproxy --mode socks5 --listen-port 9000

6. Start Brave in proxied mode and with the newly-added command-line parameter:

       npm run start -- --proxy-server="socks5://localhost:9000" --safebrowsing-manual-extension-blocklist=jknemblkbdhdcpllfgbfekkdciegfboi

7. Make sure that the `ChromeExtMalware` list in `brave://safe-browsing/#tab-db-manager` was downloaded successfully and has a non-zero size.
8. Go to the Chrome Web Store to install this [manually-flagged extension](https://chrome.google.com/webstore/detail/suspicious-site-reporter/jknemblkbdhdcpllfgbfekkdciegfboi).

Among the many requests, you should see a `POST` request to `safebrowsing2.brave.com`. If it's a 200, then it was successful and the extension should be disabled and flagged as malware in the UI (look in `brave://extensions`).

# Brave list

In some cases however, when Brave becomes aware of a potential harms to users caused by an extension and that extension is not removed from the [Chrome Web Store](https://chrome.google.com/webstore/category/extensions), Brave may decide to block it independently of Safe Browsing / Google.

We will generally do this only in these cases:

1. We have evidence that the extension handles credentials or access tokens without properly requesting permission from the user.
2. The site that the extension is modifying asks us directly to take it down and we determine that on balance, doing so would benefit users.

The current list of blocked extensions, along with a summary of the justification can be found in the `blacklist` section of <https://github.com/brave/extension-whitelist/blame/master/data/whitelist.json>.

Extension authors wanting to dispute our decisions may email <mailto:security@brave.com>.