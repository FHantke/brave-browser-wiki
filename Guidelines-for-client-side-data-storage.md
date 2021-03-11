When storing sensitive data in the browser, follow these rules:

1. Private keys, auth tokens / cookies, and passwords / passphrases should be stored encrypted using OSCrypt. They should never be in logging messages.
2. URLs related to browsing activity should never be stored or logged without sanitization to remove credentials, sensitive params, etc. It is safest to only store hostname if needed.
3. Hostnames / sanitized URLs, including in logs, can be stored in plaintext, but must be cleared from disk when the user clears browser history, including if they set the browser to automatically clear history on exit.
4. PII such as email addresses require a privacy review ticket at https://github.com/brave/security assigned to Brave's Data Protection Officer.

In addition:

* None of the above should be sent to a server without clear user consent and showing the user a link to the applicable privacy policy.
* None of the above should be stored from incognito, including Tor, sessions.