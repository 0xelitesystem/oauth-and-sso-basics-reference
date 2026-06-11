# SAML vs OIDC and common mistakes

SAML and OIDC are both single sign-on standards: SAML is the older, XML-based protocol common in enterprise and corporate apps, while OIDC is the newer, JSON-and-JWT-based protocol built on OAuth 2.0 and common in modern and mobile apps. They solve the same problem differently, and most OAuth and SSO security bugs come from a short list of avoidable mistakes.

## SAML in brief

SAML (Security Assertion Markup Language) predates OIDC and is widely deployed in enterprise environments. It uses XML documents called assertions, exchanged between an identity provider and a service provider, typically through browser redirects and form posts. SAML handles authentication and attribute sharing but is heavier than OIDC and not designed for native mobile or single-page apps. It remains common because so much corporate software already supports it, so you will still encounter it when integrating with enterprise identity systems.

## SAML vs OIDC

The practical differences: SAML uses XML and signed assertions, while OIDC uses JSON and signed JWTs. SAML is built around browser-based web SSO; OIDC, sitting on OAuth, fits web, mobile, and API access cleanly. OIDC is generally simpler to implement for modern apps and gives you OAuth access tokens for APIs in the same flow. The rule of thumb: choose OIDC for new applications, especially anything mobile or API-driven, and be ready to support SAML when integrating with established enterprise identity providers.

## Common mistakes

The frequent errors are well known. Using plain OAuth for authentication instead of OIDC, treating an access token as proof of identity. Skipping the state parameter and opening the flow to CSRF. Not validating the ID token's signature, issuer, audience, and expiry. Allowing loose redirect URI matching so codes can be hijacked. Storing tokens insecurely, especially putting refresh tokens in browser-accessible storage. Each of these reintroduces an attack the protocols were designed to prevent.

## Doing it safely

Safe integration is mostly discipline on the basics. Use the authorization code flow with PKCE for every client type. Use OIDC, not bare OAuth, when you need to know who the user is, and validate the ID token fully. Keep access tokens short-lived, store refresh tokens only where they cannot be stolen, and rotate them. Validate state and use exact redirect URIs. Request the minimum scopes. None of this is exotic; the protocols are secure when used as specified, and most breaches trace back to skipping one of these steps.
