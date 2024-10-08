# When is a security review needed?

If a code change (pull request, commit, etc.) satisfies ANY of the following, it requires a security review before it can be merged (even if behind a flag or only for certain channels). We prefer to review things at the design/spec stage and then again at the implementation stage.

1. It is a major feature that touches many parts of the code or required work from multiple teams.
2. It modifies or adds network requests, or it rewrites URLs (including internal requests like those with the `brave://` scheme).
3. It is related to money/BAT.
4. It involves cryptography (including anything which generates a random number, a random series of bytes, or the like) or distributes cryptography that is not fully open source.
5. It adds new dependencies (e.g. Docker images), integrations, or plugins. Update (9/2023): Please choose "Third-Party Dependency Review" when opening these in brave/reviews which requires a security review and principal engineer review in brave-core.
6. It is related to sensitive user information such as cookies, passwords, and private browsing data.
7. It changes the amount of data collected by Brave or one of its partners  — including making any logs which may be sent to Brave or a third party.
8. It adds a new extension that is built-in or has special privileges in Brave.
9. It changes any security/privacy messaging in our products (warning messages, security icons, etc.) or introduces non-trivial changes to any security/privacy-related UI elements, such as the URL bar (which displays security state and origin), the certificate viewer, interstitial pages, devtools, and shields.
10. It adds a new channel or modifies an existing channel for distributing software produced by Brave, including software updates.
11. It has to do with a new window API. Please see https://docs.google.com/document/d/1BfJbLCvwToPqXHzsKqQgDuluwVDtGhzFR1BclqVzqaA/edit first.
12. Adds any "content scripts" which run in the main world or an isolated world, AKA any scripts which interact with in-page JavaScript.
13. It adds a new way of collecting data from A/B tests (not through Griffin or Crash Reports).
14. It saves data on a user's device and later retrieves it for purposes that are not strictly necessary (such as analytics).
15. It involves adding or updating mojo APIs / IPC, or adding new WebUIs such as chrome:// or chrome-untrusted:// pages.

If you are not sure whether you need a security review, you should probably ask for one just to play it safe.

# How to request a security review.

Security reviews can be filed by staff here: https://github.com/brave/security/issues

Outside contributors should ask a staff to file a security issue for them if needed.