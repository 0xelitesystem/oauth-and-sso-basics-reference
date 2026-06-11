# Authorization code flow with PKCE

The authorization code flow with PKCE is the recommended OAuth 2.0 flow for almost all clients today. The app sends the user to the authorization server to log in, gets back a short-lived authorization code over a redirect, then exchanges that code for tokens in a direct back-channel call. PKCE adds a one-time secret that proves the same app that started the flow is the one redeeming the code.

## Why PKCE exists

PKCE (Proof Key for Code Exchange, pronounced "pixy") closes a real attack: if someone intercepts the authorization code as it comes back through the redirect, they could try to exchange it for tokens themselves. PKCE prevents this by binding the code to a secret only the original app knows. It was originally designed for mobile apps but is now recommended for all client types, including server-side and single-page apps, because it strengthens the flow at no real cost.

## The PKCE pieces

Before starting, the client generates a random secret called the code verifier and computes its hash, the code challenge (using SHA-256, the recommended method). The client sends the code challenge when it kicks off the flow, and later sends the raw code verifier when it exchanges the code for tokens. The authorization server hashes the verifier and checks it matches the challenge it stored. Because only the original client knows the verifier, a stolen code is useless without it.

## The flow step by step

First, the client redirects the user's browser to the authorization server's authorize endpoint, including its client ID, the requested scopes, a redirect URI, a random state value, and the code challenge. The user authenticates and consents. The authorization server redirects back to the client's redirect URI with a short-lived authorization code (and the state). The client then makes a direct server-to-server POST to the token endpoint, sending the code, its client ID, the redirect URI, and the code verifier. The server validates everything and returns the tokens.

## State and redirect safety

Two checks protect the flow. The state parameter is a random value the client sends out and verifies on return, which prevents cross-site request forgery by ensuring the response matches a request this client actually started. The redirect URI must exactly match one pre-registered with the authorization server, so codes cannot be redirected to an attacker's site. Skipping state validation or allowing loose redirect URI matching are classic mistakes that reopen the attacks the flow is designed to prevent.
