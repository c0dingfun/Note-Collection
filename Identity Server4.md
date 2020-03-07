# [Identity Server 4 Concepts](https://identityserver4.readthedocs.io/_/downloads/en/latest/pdf/)

- User - the human being the own of resources to be protected.
- Client - the software requests from IdentityServer4 the user's identity (via identity token) or/and for accessing a protected resource (via access token)
- Note: client also has identity (Client Identity) and must be register with IdentityServer4 before it can request tokens.
- Example of clients are web application, native mobile or desktop application, SPA, and server process, etc.
- Resources are something you want to protect with IdentityServer4 such as **identity data** and APIs.
- Identity data is identity information (ask claims) about a user: name or email address.
- APIs is API resources represent functionality a client want invoke--like the WebAPI.
- **Identity Token** represents the outcome of an **Authentication** process. It contains at a bare minimum an identifier for a user (called **sub** aka subject claim) and information about how and whne the user authenticated. It can contain additional identity data.
- **Access Token** allows access to an API resource. Clients request access tokens and forward them to the API. Access tokens contain information about the client and the user (if present). APIs use that information to authorize access to their data.
