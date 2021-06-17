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
		- 