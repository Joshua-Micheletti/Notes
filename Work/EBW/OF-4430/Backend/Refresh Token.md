There needs to be a system which detects when an HTTP call receives an invalid session or expired session response and uses the refresh token to retrieve the updated token and the updated refresh token.

Every HTTP request needs to be able to send back the updated token to the frontend.

The HttpRequest util needs to be able to detect the invalid session, if that's the case:
- try to obtain a new token with the refresh token
	- if that succeeds, append the new token to the response and repeat the previous http request
	- if that fails, return an error but include the information that the session expired in the response

## Trimble documentation
In case the access_token expires, you can use the refresh_token obtained in **Step 3** to fetch a new access_token, refresh token pair.  The headers and the POST parameters required to hit the token endpoint provided below:

**POST** [https://id.trimble.com/oauth/token  
](https://id.trimble.com/oauth/token)
**Headers:**  
- Authorization: Basic “ClientID:ClientSecret (base64 encoded)”  
- Content-Type: application/x-www-form-urlencoded  
- Accept: application/json

**Request body:**  
- grant_type=refresh_token // This is a static value  
- refresh_token=refresh_token // from **Step 3**