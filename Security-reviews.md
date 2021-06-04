# When is a security review needed?

If a code change (pull request, commit, etc.) satisfies ANY of the following, it requires a security review before it can be merged:

1. It is a feature important enough that there has been at least one meeting about it.
2. It modifies or adds network requests.
3. It is related to money/BAT.
4. It involves cryptography, including anything which generates a random number, a random series of bytes, or the like.
5. It adds new dependencies, integrations, or plugins.
6. It is related to sensitive user information such as cookies, passwords, and private browsing data.
7. It changes the amount of data collected by Brave or one of its partners  — including making any logs which may be sent to Brave or a third party.
8. It adds a new extension to the list of extensions which Brave doesn't warn before installing.
9. It changes any security/privacy messaging in our products (warning messages, security icons, etc.).
10. It adds a new channel or modifies an existing channel for distributing software produced by Brave, including software updates.

If you are not sure whether you need a security review, you should probably ask for one just to play it safe.

# How to request a security review.

Security reviews can be filed by staff here: https://github.com/brave/security/issues

Outside contributors should ask a staff to file a security issue for them if needed.