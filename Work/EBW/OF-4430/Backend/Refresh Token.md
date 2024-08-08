There needs to be a system which detects when an HTTP call receives an invalid session or expired session response and uses the refresh token to retrieve the updated token and the updated refresh token (if the refresh token changes).

Every HTTP request needs to be able to send back the updated token to the frontend.

The HTT