## AC SKU Security / Privacy Model

### Parties: 
* Brave Browser Users
* Brave operators
* Publishers/creators
* Custodian (Uphold or Gemini)

### Goals: 
* Allow the user to contribute to publishers in proportion to their attention without revealing their browsing history in a linkable way with user funds and anonymous card funds
* Prevent auto-contribute from being under-funded ( a user should not be able to contribute more funds than were transferred to Brave )
* Allow us to distinguish between user and anonymous card funds
* Eliminate the usage of anonize in favor of a privacy pass based protocol
* Lay the groundwork for the full featured SKUs product

### Resources:
* AWS
* Kafka Cluster and Voting Topic
* Payment Service
* Challenge Bypass Service
* Eyeshade Service
* Uphold Cards and Services
* Internet
* User computers, Brave browser installation with stored secrets

### Threat Model:
* DDoS against publicly exposed endpoints ( payments service )
* Server secrets being leaked, the signing key, database credentials, etc
* User sending funds with the wrong amount or to the wrong destination ( e.g. themselves )
* “Replaying” the same transaction against multiple different orders
* Brave using transaction information to identify the user
* Malicious user modifying voting payload
* Replay of already redeemed voting tokens
* Unauthorized access to orders and transactions


### Attack Surfaces:
* Macaroon token ( e.g. really long or otherwise malformed )
* Funding source for transactions (Uphold) 
* Publicly exposed endpoints ( payments service )
* MITM - between user and payment service, payment service and challenge bypass
* Kafka cluster access
* Database access

### Out of Scope:
* Cloud provider level access
* Compromise of a user’s device
* Malicious extension/plugin might be able to read tokens from sqlite database the browser uses to store tokens if file level access is given to the extension/plugin, allowing potentially votes to be cast by said extension/plugin using user’s tokens.
* MITM ( Upstream ISP level TLS compromise could leak payload which tells who a user is voting for )

### Protocol:

The user funded auto-contribute protocol at a high level consists of the user transferring some amount of funds to Brave, whose payment service issues voting tokens and tallies the resulting redeemed votes sent by the client. The issuance and redemption consists of a round of the Privacy Pass cryptographic protocol, of which you can find a brief description [here](https://docs.rs/challenge-bypass-ristretto/1.0.0-pre.0/challenge_bypass_ristretto/#cryptographic-protocol).

#### Client / Server One Time Setup:

* The creation of the SKU token for AC SKU
* Issuers key is created
* Suggestions Schema modified to include Order ID
* Client pins the payment service TLS certificate
* The cards used to receive the funds from auto-contribute will be created with an anonymous address so Brave cannot look up user information
* Service Level Protections
  * Rate limiting on API calls
  * Panic traces are not exposed to the end user
  * Body read limiters
  * Endpoint input validation is performed as appropriate
  * Kafka uses mutual TLS authentication
  * Databases all connect over TLS with a user + secret password
  * Security groups limit access to databases, kafka, challenge bypass service to only private network access
  * Full SKU functionality will initially be behind a feature flag, the only SKU token accepted will be hardcoded to the AC SKU token
  * Consider pinning the challenge bypass cert or switching to mutual TLS authentication
 
#### Monthly:

1. Once a month a user’s browser who has auto-contribute enable will begin the auto-contribute process
1. The browser will interact with the SKUs endpoint to create an order for an integral number of auto-contribute votes
1. Upon creating the order the browser will effect a transfer to the designated Brave owned wallet via the anonymous address depending on the funding source ( user funds or anonymous card funds )
1. In the case of user funds, the browser will send the completed transaction identifier to the payments service.
1. The payments service will confirm that the amount transferred, destination of the funds, and the token is correct. The external transaction identifier will be recorded against this particular order so it cannot be reused. The order will then be marked as paid.
1. The browser will verify the order is paid and request credentials for the order. 
1. The payment service will check the status of the order and provide credentials for the browser to sign the votes
1. The voting events will be sent by the Browser through packaging up a number of token redemptions, and sent along with the channel for the votes to be applied to the /v1/vote/ endpoint on the payment service
1. The handler within the endpoint will take the credential tokens within the payload and validate them by sending them to the challenge bypass microservice which records the token identifiers so they cannot be double spent.
1. For each valid credential a vote will be cast based on the channel that is specified within the payload, protecting against unvalidated inputs.
1. Votes are cast to eyeshade service through kafka “vote” topic, produced by payments service and consumed by eyeshade service for ledger entry.
1. Once a month the aggregate votes are used to calculate how the funds accumulated in the auto contribute wallets should be split among publishers

These voting token redemption events or “votes” should not be linkable to the user to whom they were issued provided:

1. There is a sufficient anonymity set formed by users who are undergoing auto-contribute.
1. The time between token issuance and confirmation is not predictable.
1. The vote metadata does not include information unique to the user (that is not otherwise unique to this event.)
1. The user is not made re-identifiable through other side channels such as IP address information. We ensure this by configuring our CDN to neither log nor forward client address information.
1. The signing keys used to sign voting tokens are widely used, we do not use unique keys for small user cohorts. To this effect we publish all signing keys publicly and instruct the browser to verify DLEQ proofs.

#### Timing Signals:
1. Order creation ( unlinked to any IDs at this point, returns order id )
1. Transaction submission (linked to the order id, is linkable to server to uphold anonymous card or user wallet that is submitting funds )
1. Credential creation ( linked to the order id )
1. Credential retrieval ( linked to credential creation, thus order id )
1. Vote submission ( Unlinkable to anything except the funding source )