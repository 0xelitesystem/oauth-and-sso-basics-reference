# Tokens and scopes

OAuth and OpenID Connect deal in three kinds of tokens: an access token that grants access to APIs, a refresh token that obtains new access tokens without re-prompting the user, and an ID token that tells the client who the user is. Scopes are the labels that define exactly what access a token grants, so the user consents to a specific, limited set of permissions.

## Access tokens

An access token is the credential the client sends to the resource server to call protected APIs. It is meant to be short-lived (minutes to an hour) so that if it leaks, the exposure is limited. The client treats it as opaque: it does not need to read it, just present it. The resource server validates it and checks that its scopes cover the requested action. Because access tokens are bearer tokens (whoever holds one can use it), keeping them short-lived and out of insecure storage matters.

## Refresh tokens

A refresh token lets the client get a new access token when the old one expires, without making the user log in again. It is longer-lived and far more sensitive, because it can mint new access tokens, so it must be stored securely and never exposed to a browser front-end where it could be stolen. Authorization servers often rotate refresh tokens (issuing a new one each use and invalidating the old) to limit damage if one leaks. Refresh tokens are why a logged-in session can persist smoothly while individual access tokens stay short.

## ID tokens

An ID token is part of OpenID Connect, not plain OAuth. It is a signed JWT that asserts the user's identity to the client: who they are, when they authenticated, and which authorization server issued the claim. Crucially, the ID token is for the client to consume, to establish who logged in, whereas the access token is for the resource server. Mixing these up (sending an ID token to an API, or trying to read identity from an access token) is a common error; each token has its own intended audience.

## Scopes

Scopes are strings that define the specific permissions a token grants, such as read access to a profile or write access to a calendar. The client requests the scopes it needs, the user sees and consents to them, and the issued token is limited to exactly those. Scopes are the mechanism behind least privilege in OAuth: request only what you need, and the resource server enforces that the token's scopes cover each call. Over-requesting scopes erodes user trust and increases the damage if a token leaks, so keep scope requests minimal.
