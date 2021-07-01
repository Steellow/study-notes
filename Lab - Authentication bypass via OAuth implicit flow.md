<-- [[OAuth 2.0 authentication vulnerabilities]]
🌍 https://portswigger.net/web-security/oauth/lab-oauth-authentication-bypass-via-oauth-implicit-flow
# Lab: Authentication bypass via OAuth implicit flow
- Uses OAuth service to allow users to login with their social media account
- Flawed validation by client allows to log in to other users' accounts without their password
- Credentials
	- email: `carlos@carlos-montoya.net`
	- some: `wiener:peter`

## Notes
- [Setting up Burp with FoxyProxy](https://blog.nvisium.com/setting-up-burpsuite-with-firefox-and)
	- BurpSuite is a proxy = web traffic will be forwarded from browser through Burp
	- We can see the HTTP req & res, and manipulate it