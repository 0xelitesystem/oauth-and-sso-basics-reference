# OIDC and single sign-on

OpenID Connect (OIDC) is a thin identity layer built on top of OAuth 2.0 that lets an application verify who a user is, not just access their data. It reuses the same authorization code flow but adds an ID token and a standard way to fetch user profile information. Single sign-on is the experience this enables: logging in once at an identity provider and being recognized across many applications.

## What OIDC adds

Plain OAuth 2.0 is about authorization (delegated access), not authentication (proving identity), so using OAuth alone to log users in is a known anti-pattern. OIDC fills that gap. It defines the ID token, a signed JWT containing identity claims like the user's unique identifier (the `sub` claim) and authentication time; a standard `openid` scope to request it; and a userinfo endpoint to fetch profile details. With OIDC, the same flow that grants API access can reliably answer "who is this user," which is what login actually requires.

## How OIDC reuses OAuth

OIDC does not replace OAuth; it sits on it. The client runs the authorization code flow with PKCE exactly as before, but includes the `openid` scope. The authorization server (now also called an OpenID Provider) returns an ID token alongside the access token. The client validates the ID token's signature, issuer, audience, and expiry, then trusts its claims about the user. Because it is the same flow plus an identity token, supporting OIDC is mostly a matter of requesting the right scope and validating the ID token correctly.

## How single sign-on works

Single sign-on means a user authenticates once with a central identity provider and is then automatically recognized by other applications that trust that provider. When you visit a second app, it redirects you to the identity provider; if you already have an active session there, the provider issues a token without asking you to log in again, and you land back at the app logged in. The applications never see your password; they trust the identity provider's signed assertion. One login, many apps, all relying on the same trusted source.

## Validating the ID token

The security of OIDC login rests on validating the ID token, and skipping steps here is dangerous. The client must verify the token's signature against the provider's published keys, confirm the issuer matches the expected provider, confirm the audience is this client, and check that it has not expired. It should also check the nonce it sent to defend against replay. An unvalidated ID token is just a claim anyone could forge; validation is what turns it into trustworthy proof of identity.
