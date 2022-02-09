Chrome extensions are generally available in Brave, at the user's risk. These are not reviewed by Brave and user should be cautious.

In some cases however, when Brave becomes aware of a potential harms to users caused by an extension and that extension is not removed from the [Chrome Web Store](https://chrome.google.com/webstore/category/extensions), Brave may decide to block it independently.

We will generally do this only in these cases:

1. We have evidence that the extension handles credentials or access tokens without properly requesting permission from the user.
2. The site that the extension is modifying asks us directly to take it down and we determine that on balance, doing so would benefit users.

The current list of blocked extensions, along with a summary of the justification can be found in the `blacklist` section of <https://github.com/brave/extension-whitelist/blob/master/data/whitelist.json>.

Extension authors wanting to dispute our decision may email <mailto:security@brave.com>.