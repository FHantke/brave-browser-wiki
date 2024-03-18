# When is a security review needed?

If a code change (pull request, commit, etc.) satisfies ANY of the following, it requires a security review before it can be merged:

1. It is a feature important enough that there has been at least one meeting about it.
2. It modifies or adds network requests.
3. It is related to money/BAT.
4. It involves cryptography (including anything which generates a random number, a random series of bytes, or the like) or distributes cryptography that is not fully open source.
5. It adds new dependencies (e.g. Docker images), integrations, or plugins. Update (9/2023): Please choose "Third-Party Dependency Review" when opening these in brave/reviews.
6. It is related to sensitive user information such as cookies, passwords, and private browsing data.
7. It changes the amount of data collected by Brave or one of its partners  — including making any logs which may be sent to Brave or a third party.
8. It adds a new extension that is built-in or has special privileges in Brave.
9. It changes any security/privacy messaging in our products (warning messages, security icons, etc.).
10. It adds a new channel or modifies an existing channel for distributing software produced by Brave, including software updates.
11. It modifies handling of URL protocols, including internal urls like brave:// (especially any rewriting of internals urls). 
12. It has to do with a new window API. Please see https://docs.google.com/document/d/1BfJbLCvwToPqXHzsKqQgDuluwVDtGhzFR1BclqVzqaA/edit first.
13. Adds any "content scripts" which run in the main world or an isolated world, AKA any scripts which interact with in-page JavaScript.
14. It adds a new way of collecting data from A/B tests (not through Griffin or Crash Reports).
15. It saves data on a user's device and later retrieves it for purposes that are not strictly necessary (such as analytics).

If you are not sure whether you need a security review, you should probably ask for one just to play it safe.

# How to request a security review.

Security reviews can be filed by staff here: https://github.com/brave/security/issues

Outside contributors should ask a staff to file a security issue for them if needed.