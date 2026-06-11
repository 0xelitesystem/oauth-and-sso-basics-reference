# OAuth and SSO Basics

A plain-language reference on OAuth 2.0 and single sign-on: the roles involved, the authorization code flow with PKCE, the three kinds of tokens, scopes, OpenID Connect, how SAML and OIDC differ, and the common mistakes. It is aimed at someone integrating login or delegated access who wants an accurate mental model of what is actually happening.

## The core idea

OAuth 2.0 is a delegation protocol: it lets a user grant an application limited access to their data on another service without sharing their password, by having an authorization server issue the application a scoped access token. The modern recommended flow is the authorization code flow with PKCE, where the app redirects the user to log in, receives a short-lived code, and exchanges it for tokens. OpenID Connect adds an identity layer on top of OAuth so the same flow can also tell the app who the user is, which is what powers single sign-on.

## What is inside

- `01-roles-and-the-problem.md` what OAuth solves and the four roles.
- `02-authorization-code-flow-with-pkce.md` the recommended flow, step by step.
- `03-tokens-and-scopes.md` access, refresh, and ID tokens, plus scopes.
- `04-oidc-and-sso.md` OpenID Connect and how single sign-on works.
- `05-saml-vs-oidc-and-mistakes.md` SAML in brief and common errors to avoid.

## The stance

This reference treats OAuth and SSO as a careful exchange of trust, not a black box you bolt on. Knowing the roles, the flow, and what each token is for lets you integrate login safely and debug it when it breaks. The goal is correctness on the details that matter for security, especially PKCE and token handling.

## License

MIT. Copyright 0xelitesystem 2026.
