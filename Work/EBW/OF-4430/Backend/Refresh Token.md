There needs to be a system which detects when an HTTP call receives an invalid session or expired session response and uses the refresh token to retrieve the updated token and the updated refresh token (if the refresh token changes).

Every HTTP request needs to be able to send back the updated token to the frontend.

The HttpRequest util needs to be able to detect the invalid session, if that's the case:
- try to obtain a new token with the refresh token
	- if that succeeds, append the new token to the response and repeat the previous http request
	- if that fails, return an error but include the information that the session expired in the response