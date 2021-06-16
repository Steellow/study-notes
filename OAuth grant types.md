<--- [[OAuth 2.0 authentication vulnerabilities]]
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
- 