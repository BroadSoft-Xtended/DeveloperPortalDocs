## Obtain an access token.

In order to use the RTM API you will need a Team-One API "access token".

This token allows your application to access Team-One on behalf of a specific end-user.

The Team-One API access token is literally an [OAuth2](https://tools.ietf.org/html/rfc6749) access token.

You can create and manage access tokens for your _own_ account at <https://app.intellinote.net/rest/account/api-tokens>.

To obtain an access token for an arbitrary user's account, you may use the OAuth2 protocol, as described [here](https://app.intellinote.net/rest/content/examples#obtain-an-access-token-and-refresh-token-) and elsewhere.

(Until the access token expires or is revoked by the end-user) this is a one-time action.  The access token will allow your application to access the end-user's account across multiple sessions.
