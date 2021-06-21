<-- [[OAuth 2.0 authentication vulnerabilities]]  
🌍 https://portswigger.net/web-security/oauth/grant-types
# OAuth grant types
## What is an OAuth grant type?
- Specifies steps which are in OAuth process
- Also affects how client app communicates with OAuth service at each stage
	- Including how the access token is sent
- Grants are also known as "OAuth flows"
- OAuth service must be configured to support particular type of grant type
- There are several different grant types
	- Each with different level of complexity and security
	- "Authorization code" and "implicit" are the most common ones

## OAuth scopes
- For any grant type, client app has to specify which data it wants and what operation it will perform
	- This happens using `scope` paramenter
- Scopes are unique for each OAuth service
	- Even the format can change a lot
		- Some are similiar to REST API
		- Few examples:
			- `scope=contacts
			- `scope=contacts.read`  
			- `scope=contact-list-r`  
			- `scope=https://oauth-authorization-server.com/auth/scopes/user/contacts.readonly`
		- When OAuth is used for authentication, standard OpenID Connect scopes are used

## Authorization code grant type
- Short version
	1. Client app and OAuth service use redirects to exchange browser-based HTTP reqs that initiate the flow
	2. Used is asked for access
	3. Client is granted an **authorizatino code**
	4. Client exchanges this code with OAuth service to receive **access token**
	5. Client can use the access code to make API calls
- Everything after code/token exchange is sent server-2-server over a secure back-channel
	- Invisible to the end user
	- This secure channel is created when client first registers with the OAuth service
		- `client_secret` is also generated, which client uses to auth itself when sending server-2-server reqs
	- The most sensitive data (access token & user data) is not sent via browser
		- Most secure grant type
		- Should ideally be used

![Flow for the OAuth authorization code grant type](https://portswigger.net/web-security/images/oauth-authorization-code-flow.jpg)

Long version:
### 1. Authorization request
- Client sends req to OAuth service's `/authorization` endpoint
	- Asking permission to access specific user daata
	- Endpoint may vary
		- PortSwigger labs use `/auth`
		- Should be easily identifiable
- eg. `GET /authorization?client\_id=12345&redirect\_uri=https://client-app.com/callback&response\_type=code&scope=openid%20profile&state=ae13d489bd00e3c24`
	- `client_id`
		- Mandatory parameter
		- Contains unique id of client
		- The value is generated when client registers with OAuth service
	- `redirect_uri`
		- Aka. "callback URI" or "callback endpoint"
		- Many OAuth attacks are based on this
	- `response_type`
		- Determines which kind of response client is expecting
			- Meaning, which flow it wants to initiate
			- For authorization code grant type, the value is `code`
	- `scope`
		- Determines which user data the client wants
		- There can be custom scopes by OAuth provider or standard scopes by [[OpenID Connect]] specification
	- `state`
		- Stores unique, unguessable value that is tied to current session of client
		- OAuth service should return this value, with auth code
		- This parameter serves as a form of [[CSRF token]]

### 2. User login and consent
- When auth server receives the initial req, it will redirect user to login page
- User will be listed of data the client wants to access
	- This is bvased on the scope
- Once the user has approved given scope, this step will be automatic as long as the user has valid session with OAuth service

### 3. Authorization code grant
- Browser will be redirected to what was specified in the `redirect_uri`
- The resulting `GET` req will contain the auth code
	- Depending on the conf, might also include `state` parameter

### 4. Access token request
- Once the client gets the auth code, it needs to exchange it for an access token
- To do this, it send server-2-server `POST` req to OAuth service's `/token` endpoint
	- All communication from this piont takes place in secure back-channgel
		- Therefore, can't (usually) be observed or controller by attacker
- `client_id=12345&client_secret=SECRET&redirect_uri=https://client-app.com/callback&grant_type=authorization_code&code=a1b2c3d4e5f6g7h8`
	- Two new parameters
		- `client_secret`: Client must authenticate itself with secret key it got from registering with OAuth service
		- `grant_type`: Used to make sure the new endpoint knows which grant type the client wants to use. In this case, it's `authorization_code`

### 5. Access token grant
- OAuth service will validate the access token request
- If everything is ok, the server respons by granting the client an access token with the requested scope
```
{  
 "access\_token": "z0y9x8w7v6u5",  
 "token\_type": "Bearer",  
 "expires\_in": 3600,  
 "scope": "openid profile",  
 …  
}
```

### 6. API call
- Now the client can fetch user's data with API calls
- The access token is submitted in `Authorization: Bearer` header

### 7. Resource grant
- The server validates the token, and responds with requested data

# Implicit grant type
- Much simpler one
- Instead of exchanching auth code for access token, client immediately gets access token after user has give consent
- Why clients don't always use implicit grant type?
	- It's far less secure
		- All communication happens via browser redirects
		- No secure back-channel
		- = access token and user's data are more exposed to attacks
- More suited for single-page apps and native desktop apps
	- They can't easily store `client_secret` on the backend
		- Don't benefit as much from authorization code grant type
![Flow for the OAuth implicit grant type](https://portswigger.net/web-security/images/oauth-implicit-flow.jpg)

### 1. Authorization request
- Starts almost the same way as authorization code flow
- Difference: `response_type` parameter must be `token`
- `GET /authorization?client\_id=12345&redirect\_uri=https://client-app.com/callback&response\_type=token&scope=openid%20profile&state=ae13d489bd00e3c24`

### 2. User login an consent
- Self-explanatory, same as in authorization code flow

### 3. Access token grant
- This is where things start to differ
- OAuth service will redirect user's browser to `redirect_uri`
	- **Instead of sending containing auth vode, it contains accees token and ohet token-specific data as a URL fragment**!
- `GET /callback#access\_token=z0y9x8w7v6u5&token\_type=Bearer&expires\_in=5000&scope=openid%20profile&state=ae13d489bd00e3c24`
- The access token is sent in URL fragment, thus client needs some kind of script to parse and store it

### 4. API call
- Once the access token is extracted, it can be used to make API calls to OAUth service's `/userinfo` endpoint
- Unlike in authorization code flow, this also happens via browser

### 5. Resource grant
- Resource server verifies token and respons with requested resource