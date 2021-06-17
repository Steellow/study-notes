🌎 https://portswigger.net/web-security/oauth

# OAuth 2.0 authentication vulnerabilities
-   Logging with social media account is usually built with OAuth 2,.0
-   OAuth 2.0 is interesting for attackers
    -   Extremely common
    -   Prone to implementation mistakes
 
## What is OAuth?
-   Authorization framework that enables websites to request limited access to use account on some another service
    -   e.g. social media login methods
    -   Doesn't expose login credentials
-   Widely used to get certain data from another service
    -   e.g. application might use OAuth to request access to your email contacts list so that it can suggest people to connect with
-   OAuth 2.0 is the current standard, some websites still use the legacy version 1a
-   OAuth 2.0 was written from scratch rather than being developed directly from OAuth 1.0
-   As a result, the two are very different
-   **The term "OAuth" refers exclusively to OAuth 2.0 throughout these materials**

## How does OAuth 2.0 work?
-   Originally developed to share specific data between 2 apps
-   Works by defining series of interactions between client app, resource owner and the OAauth service provider
    -   **Client app** \- Website/application that wants to access user data
    -   **Resource owner** \- The user whose data the client wants
    -   **OAuth service provider** \- Website/app that controls the user's data
        -   OAuth is supported by providing an API for interacting with both auth server and resource server
-   OAuth can be implemented in different ways
    -   Known as OAuth 'flows' or 'grant types'
    -   In this topic we focus on 'authorization code' and 'implicit' flows
        -   They are the most common
        -   Brodaly, here are steps for both of those methods:
            1.  Client app requests specific user's data, telling which granty type they use and what kind of access they want
            2.  User is prompted to log in to OAuth service and add access
            3.  Client app recieves unique access token which proves that the user gave access to requested data
                -   Exactly how this happens varies on the grand type
            4.  Client app uses the access token to make API calls to the resource server

> 📃 [[OAuth grant types]]
### OAuth authentication