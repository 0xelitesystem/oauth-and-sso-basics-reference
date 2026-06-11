# Roles and the problem

OAuth 2.0 solves the problem of letting an application access a user's data on another service without the user handing over their password. It does this through four roles: the resource owner (the user), the client (the application requesting access), the authorization server (which authenticates the user and issues tokens), and the resource server (which holds the data and accepts tokens).

## The problem OAuth solves

Before OAuth, the only way to let an app act on your behalf at another service was to give it your username and password, which meant the app could do anything you could, forever, and you could not revoke it without changing your password. OAuth replaces that with delegated, limited, revocable access: the app gets a token that grants only specific permissions and can be revoked independently. The user never shares their password with the third-party app. This is the core value of the protocol.

## The four roles

The resource owner is the user who owns the data and grants permission. The client is the application that wants access, such as a web or mobile app. The authorization server is the system that authenticates the user and, with their consent, issues tokens; it is operated by the service that holds the user's account. The resource server is the API holding the protected data, which accepts and validates access tokens. Sometimes the authorization server and resource server are the same system, but the roles remain distinct.

## Confidential vs public clients

Clients come in two kinds, and the distinction drives security choices. A confidential client (a server-side app) can keep a secret credential private, so it can authenticate itself to the authorization server. A public client (a single-page app or mobile app) runs where its code is visible to the user and cannot keep a secret. Public clients therefore must not rely on a client secret, which is exactly why PKCE exists: to secure flows for clients that cannot hold a secret safely.

## Consent and revocation

A defining feature of OAuth is explicit, scoped consent. When the app requests access, the authorization server shows the user what is being requested and asks them to approve it. The user grants only what they agree to, and they can later revoke that grant without affecting their password or other apps. This combination of informed consent, limited scope, and independent revocation is what makes delegated access safe, and it is the reason OAuth became the standard for "log in with" and API access.
