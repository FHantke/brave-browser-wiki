# Security / privacy model for ads-confirmations

## Application

### Parties

* Users running Brave browser
* Advertisers
* Brave operators

### Resources

* Internet
* User computers, Brave browser installation with stored secrets
* Challenge bypass microservice
* Ads server
* Ledger server (wallet service)
* Eyeshade server (accounting service)

### Goal

* Given a Brave user observes an advertisement, provide a channel for "confirming" said viewing without revealing to Brave the particular user involved.
* Upon receiving a valid confirmation, grant the user a redeemable payment token that is worth ~70% of the observation price charged to the advertiser.
* Allow a user to redeem their payment tokens, this redemption should be unlinkable to the original confirmation events.
* Pay out for redeemed tokens on-demand after offering a notification to the user to claim their earnings.
* Prevent double spend attacks by ensuring each payment token can only be spent once.

### Threat model

* MITM on internet between user and ads server
* Crash / power loss
* Timing based loss of privacy

### Attack surfaces

* HTTPS from the world over internet to CDN
* HTTPS from CDN to the ads server
* HTTPS from the ads server to the challenge bypass microservice
* HTTPS from the ads server to the eyeshade server
* HTTPS from the ads server to the ledger server

**Out of scope:**

* Compromise of a user's device
* Brave collusion with CDN to track users based on IP

### Protocol

The ads confirmation protocol is composed of two separated rounds of the Privacy Pass cryptographic
protocol, of which you can find a brief description [here](https://docs.rs/challenge-bypass-ristretto/1.0.0-pre.0/challenge_bypass_ristretto/#cryptographic-protocol).

#### Server Setup

1. The ads server communicates with the challenge bypass microservice to create
   an issuer (consisting of a particular [`SigningKey`]) for confirmations.
1. The ads server communicates with the challenge bypass microservice to create
   a small number of issuers for payment. Each key corresponds to
   a particular payout value, for example there could be keys for 0.1, 0.3, 0.5, 1, 3, and 5 BAT.
1. The corresponding [`PublicKey`]s for the confirmations and payments
   are published in the ads catalog.

#### Client Setup

1. A user generates a pool of [`Token`]s, blinds them and submits a HTTP signed
   request to the ads server to have them signed by the server's confirmation
   [`SigningKey`].
1. The ads server makes a request to the ledger server to retrieve the wallet
   public key, checks the signature over the request and finally signs the
   tokens. Furthermore a [`BatchDLEQProof`] is created demonstrating that
   the tokens were signed by a [`SigningKey`] whose [`PublicKey`] was
   previously published in the catalog.
1. The returned [`BatchDLEQProof`] is verified and the [`SignedToken`]s are unblinded 
   and stored locally in their confirmation token pool.

The HTTP signature over this request restricts confirmation token issuance
to valid rewards wallets. A [`SigningKey`] rotation can be performed to force
clients to create a new pool of tokens.

#### Upon viewing an ad

1. A user pops an [`UnblindedToken`] from the pool, uses it
   to form a [`VerificationKey`] and uses the key to "sign" confirmation
   metadata.
1. The user forms a token redemption request consisting of the [`VerificationSignature`],
   the request metadata, the [`TokenPreimage`] associated with the [`UnblindedToken`], and
   the [`PublicKey`] corresponding to the key it was signed by.
1. The user generates a new [`Token`], and blinds it. Together with the token
   redemption request this forms a confirmation request.
1. The confirmation request is submitted to the ads server, the client 
   receives a unique identifier by which it can later retrieve a [`SignedToken`].
1. Using the included [`PublicKey`] the ads server looks up the issuer
1. The ads server forwards the token redemption request to corresponding issuer endpoint of
   the challenge bypass microservice, which marks the [`TokenPreimage`] as spent.
1. The ads server rounds the value of the confirmed event to the nearest value that
   has a corresponding payout key, then uses this key to sign the
   [`BlindedToken`]. A [`BatchDLEQProof`] is also created showing the payment token
   is signed by a [`PublicKey`] in the catalog.
1. The returned [`BatchDLEQProof`] is verified and the returned [`SignedToken`] is 
   unblinded and stored locally with the other payment tokens the user has received.

These confirmation token redemption events should not be linkable to the user to whom 
they were issued provided: 

1. There is a sufficient anonymity set formed by users who are creating confirmation tokens.
2. The time between token issuance and confirmation is not predictable.
3. The confirmation metadata does not include information unique to the user (that is not otherwise unique to this event.)
4. The user is not made re-identifiable through other side channels such as IP address information. We ensure this by configuring our CDN to neither log or forward client address information.
5. The signing keys used to sign both confirmation and payment tokens are widely used, we do not use unique keys for small user cohorts. To this effect we publish all signing keys publicly and instruct the browser to verify DLEQ proofs.

#### Weekly

1. A user takes all stored payment tokens that they have accumulated and forms
   a bulk token redemption request, submitting their wallet identifier as the 
   metadata that is "signed".
1. Using the included [`PublicKey`] for each token redemption request the looks
   up the issuers for each - and thus the corresponding value.
1. The ads server forwards the token redemption requests to corresponding issuers endpoint of
   the challenge bypass microservice, which marks the [`TokenPreimage`] of each as spent.
1. The ads server makes a request to the eyeshade server, submitting a
   request for payment for the value of each token.

#### During the first week of the month

1. A Brave operator will pull a list of balances owed to each wallet for ads
   earnings.
1. Ad earnings will be made available to users to claim

[`BlindedToken`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.UnblindedToken.html
[`SigningKey`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.SigningKey.html
[`PublicKey`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.PublicKey.html
[`Token`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.Token.html
[`SignedToken`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.SignedToken.html
[`UnblindedToken`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.UnblindedToken.html
[`VerificationKey`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.VerificationKey.html
[`VerificationSignature`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.VerificationSignature.html
[`TokenPreimage`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/voprf/struct.TokenPreimage.html
[`BatchDLEQProof`]: https://docs.rs/challenge-bypass-ristretto/latest/challenge_bypass_ristretto/dleq/struct.BatchDLEQProof.html