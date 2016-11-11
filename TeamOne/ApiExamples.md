## Obtain an API Key

In order to use the Team One REST API you must obtain an *API Key*
or *client_id* for your application.

To obtain a *client_id*, send an email to the address
[apiteam@intellinote.net](mailto:apiteam@intellinote.net?subject=API%20Key)
with the subject "API Key".

You will receive an email that assigns two values:

 * A ***client_id***, such as `lsJLcwxUyll0p`, which identifies your
  application to the Team One API.  The client_id is like a
  username--unique to your application but not necessarily a secret.

 * A ***client_secret***, such as `MkalkvoRGBZP1O`, which authenticates
  your application to the Team One API.  The client_secret is like a
  password--it should remain a "shared secret" between Team One and
  your application.

You'll use the *client_id* and *client_secret* values in order to
obtain an *access token* that allows your application to access
Team One on behalf of an end-user, as described below.

---
## Obtain an *Authorization Code*

An ***authorization code*** is proof that a given end-user has authorized
your application to act on his or her behalf.

To obtain an *authorization code*, the end-user must log-in to
Team One and grant your application access.

To achieve this, direct the end-user to the path
`/auth/oauth2/authorization` passing the following query string
parameters:

  * `response_type` - following the OAuth2 specification this must be
    the value `code`.

  * `client_id` - the ["API Key" assigned to your application](#obtain-an-api-key).

  * `scope` - a comma-delimited list of the scopes your application is
    requesting access to, e.g., `read,write`.

  * `redirect_uri` - where to return the user to after authorization
    (passing the assigned authorization token back to your application
    as a query string parameter named `code`).

For example, let's assume that:

1. Your application has been assigned the client_id `lsJLcwxUyll0p`.

2. You'd like to obtain `read` and `write` access to the user's
   account.

4. Your application is hosted at the domain `https://example.com`.

4. Your application will use the path `/oauth/callback` to receive the
   *authorization code*.

Your application must then direct the user to the URL:

```no-highlight
https://{request.url.host}{@uri path="/auth/oauth2/authorization?response_type=code&client_id=lsJLcwxUyll0p&scope=read,write&redirect_uri=https://example.com/oauth/callback"/}
```

For example, you might provide the user with a direct link in your
HTML code:

```html
<a href="https://{request.url.host}{@uri path="/auth/oauth2/authorization?response_type=code&client_id=lsJLcwxUyll0p&scope=read,write&redirect_uri=https://example.com/oauth/callback"/}">Link to Team One Account</a>
```

or your server might redirect the user to Team One by returning a
HTTP 302 response:

```http
HTTP/1.1 302 Found
Location: https://{request.url.host}{@uri path="/auth/oauth2/authorization?response_type=code&client_id=lsJLcwxUyll0p&scope=read,write&redirect_uri=https://example.com/oauth/callback"/}
```

The end-user will be presented with a form by which she can authorize
your application to access her Team One account. (Or more
specifically, access the Team One *scopes* enumerated in the query
string.)

After a succesful authorization, Team One will redirect the user's
browser back to the `redirect_uri` you provided, with the
*authorization code* added a query string parameter named `code`.

```no-highlight
https://example.com/oauth/callback?code=h4iu8zs29zy9cnmi840ub5bete3o9a4i
```

Make a note of the returned *authorization code*. (In the example
above it is `h4iu8zs29zy9cnmi840ub5bete3o9a4i`.) You'll use it below
to obtain an *access token* which ultimately provides access to the
end-user's account.

---
## Obtain an *Access Token* and *Refresh Token*

An ***access token*** is a credential that gives your application
(temporary) access to an end-user's Team One account.

A ***refresh token*** is a credential that allows your application
to obtain a new  *access token* when the current one becomes stale.

Given an [*authorization code*](#obtain-an-authorization-code-) (and
[the *client_id* and *client_secret* values assigned to your application](#obtain-an-api-key))
you can obtain an *access token* and an optional *refresh-token* by submitting a server-to-server
request that includes all three values.

### Request

Specifically, you must submit an HTTP POST to the path
`/auth/oauth2/access`, posting a JSON document with the following
attributes:

 * `code` - the *authorization code* created
   for the end-user and your application.

 * `client_id` - the *client_id* value assigned to your application.

 * `client_secret` - the *client_secret* value assigned to your application.

 * `grant_type` - following the OAuth2 specification, this must be the
   value `authorization_code`.

```javascript
var request = require("request"); // https://www.npmjs.org/package/request
var headers = { "Content-Type": "{content_type}" };
var url     = "https://{request.url.hostname}{@uri path="{path|s}"/}";
var payload = '{payload|s}';

console.log("REQUEST: POST "+ url);
request.post( url, {body:payload,headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```

### Response

If successful, the Team One server will return a JSON document
containing the *refresh_token* assigned to your application and an
active *access_token* that be used immediately for access.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "refresh_token":"a29cf572ccdb7924d2c35ca95753f233",
  "access_token":"9m74lrwlcubu766rbmkqp2srche8w7b9"
}
```

You may then use the *access token* to submit requests to the
Team One API on behalf of the end-user.

Note that for security reasons, the *access token* value is only valid
for a short time.  If you submit an API request with a "stale"
*access token*, the server will respond with an HTTP 401
("Unauthorized") status.  This indicates that you must use the *refresh token*
to obtain a new *access token*, as described below.

---
## Use *Refresh Token* to replace a stale *Access Token*

As noted, your *access token* will eventually time-out.

When this happens, you will start to recieve 401 (Authentication Required)
responses from the server.  This is your cue that it is time to refresh
the *access token*.

Given an *refresh token* (and
[the *client_id* and *client_secret* values assigned to your application](#obtain-an-api-key))
you can obtain an *access token* (and an optionally, a replacement *refresh-token*) by submitting
a server-to-server request that includes all three values.

### Request

Specifically, you must submit an HTTP POST to the path
`/auth/oauth2/access`, posting a JSON document with the following
attributes:

 * `token` - the *refresh token* created
   for the end-user and your application.

 * `client_id` - the *client_id* value assigned to your application.

 * `client_secret` - the *client_secret* value assigned to your application.

 * `grant_type` - following the OAuth2 specification, this must be the
   value `refresh_token`.

```javascript
var request = require("request"); // https://www.npmjs.org/package/request
var headers = { "Content-Type": "{content_type}" };
var url     = "https://{request.url.hostname}{@uri path="{path|s}"/}";
var payload = '{payload|s}';

console.log("REQUEST: POST "+ url);
request.post( url, {body:payload,headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```

### Response

If successful, the Team One server will return a JSON document
containing an active *access_token* that be used immediately for
access to the end-user's account.

The server *may* also return a new *refresh token* value.  In that case
you should make a note of the new *refresh token*, since the old one will
not work any more.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "refresh_token":"a29cf572ccdb7924d2c35ca95753f233",
  "access_token":"274ab5e925e4a654a94bd935844c71c6"
}
```

---
## Obtain an *Access Token* using the Implicit Workflow

The OAuth2 *implicit workflow* is an alternative to [the *authorization-token*-based process described above](#obtain-an-access-token-).

In the *implicit workflow* no persistent *refresh token* is
provided.  The end-user must authenticate to the Team One service
(and if necessary, authorize your application's access) every time an
*access token* is needed.

The *implicit workflow* is most suitable for client-side (browser or
mobile-device based) applications that (a) do not have a (secure)
server-side component at which the *client_secret* value can be
stored and from which the server-to-server request is submitted, and
(b) for which the end-user will be actively using whenever the
application needs to invoke the Team One API.

In the *implicit workflow* the *access token* is both requested and delivered by URLs exchanged between applications by re-directing the end-user's browser.

The process starts by sending the end-user to the to the path
`/auth/oauth2/authorization` passing the following query string
parameters:

  * `response_type` - following the OAuth2 specification this must be
    the value `token`.

  * `client_id` - the ["API Key" assigned to your application](#obtain-an-api-key).

  * `scope` - a comma-delimited list of the scopes your application is
    requesting access to, e.g., `read,write`.

  * `redirect_uri` - where to return the user to after authorization
    (passing the assigned *access token* back to your application
    as a query string parameter named `token`).

For example, let's assume that:

1. Your application has been assigned the client_id `lsJLcwxUyll0p`.

2. You'd like to obtain `read` and `write` access to the user's
   account.

4. Your application is hosted at the domain `https://example.com`.

4. Your application will use the path `/oauth/callback` to receive the
   *access token*.

Your application must then direct the user to the URL:

```no-highlight
https://{request.url.host}{@uri path="/auth/oauth2/authorization?response_type=token&client_id=lsJLcwxUyll0p&scope=read,write&redirect_uri=https://example.com/oauth/callback"/}
```

For example, you might provide the user with a direct link in your
HTML code:

```html
<a href="https://{request.url.host}{@uri path="/auth/oauth2/authorization?response_type=token&client_id=lsJLcwxUyll0p&scope=read,write&redirect_uri=https://example.com/oauth/callback"/}">Link to Team One Account</a>
```

or your server might redirect the user to Team One by returning a
HTTP 302 response:

```http
HTTP/1.1 302 Found
Location: https://{request.url.host}{@uri path="/auth/oauth2/authorization?response_type=token&client_id=lsJLcwxUyll0p&scope=read,write&redirect_uri=https://example.com/oauth/callback"/}
```

The end-user must authenticate and (if she hasn't done this already)
is presented with a form by which she can authorize
your application to access her Team One account. (Or more
specifically, access the Team One *scopes* enumerated in the query
string.)

After a succesful authorization, Team One will redirect the user's
browser back to the `redirect_uri` you provided, with the
*access token* added a query string parameter named `token`.

```no-highlight
https://example.com/oauth/callback?token=9m74lrwlcubu766rbmkqp2srche8w7b9
```

You may then use the *access token* to submit requests to the Team One API on behalf of the end-user.

Note that for security reasons, the *access token* value is only valid for a short time.  If you submit an API request with a "stale" *access token*, the server will respond with an HTTP 401 ("Unauthorized") status.  This indicates that you must re-submit the request above to "refresh" the token.

---

## Perform a userless (application-level) log-in

Some Team One API methods can be invoked outside of any specific "user context".

For example an application can create a new user record (via `POST /user`) without being authorized for access to any existing user's account.

If your application has been authorized (by Team One) to do so, it can peform a type of "user-less" OAuth2 authentication known as a "client_credentials" grant.  This "userless" application log-in generates an *access token* and *refresh token* like any other OAuth2 authorization process.  The resulting *access token* will allow the application to interact with the Team One API on its own behalf--independent of any user accounts.

The process for obtaining an *access token* via the `client_credentials` grant is very similiar to the process used to obtain an *access token* using the `authorization_code` and `refresh_token` grant types.

### Request

Specifically, you must submit an HTTP POST to the path
`/auth/oauth2/access`, posting a JSON document with the following
attributes:

 * `client_id` - the *client_id* value assigned to your application.

 * `client_secret` - the *client_secret* value assigned to your application.

 * `grant_type` - following the OAuth2 specification, this must be the
   value `client_credentials`.

```javascript
var request = require("request"); // https://www.npmjs.org/package/request
var headers = { "Content-Type": "{content_type}" };
var url     = "https://{request.url.hostname}{@uri path="{path|s}"/}";
var payload = '{payload|s}';

console.log("REQUEST: POST "+ url);
request.post( url, {body:payload,headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```

### Response

If successful, the Team One server will return a JSON document
containing the *refresh_token* assigned to your application and an
active *access_token* that be used immediately for access.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "refresh_token":"a29cf572ccdb7924d2c35ca95753f233",
  "access_token":"9m74lrwlcubu766rbmkqp2srche8w7b9"
}
```

You may then use the *access token* to submit requests to the
Team One API.

Note that for security reasons, the *access token* value is only valid
for a short time.  If you submit an API request with a "stale"
*access token*, the server will respond with an HTTP 401
("Unauthorized") status.  This indicates that you must use the *refresh token*
to obtain a new *access token*, as described above.

---

## List Organizations

With an active [*access_token*](#obtain-an-access-token-) you can submit an HTTP GET request to the path `/v2.0/orgs` to obtain a list of *organizations* the end-user belongs to.

### Request

```javascript
var request      = require("request"); // https://www.npmjs.org/package/request
var access_token = "{access_token}";
var headers      = { Authorization: "Bearer " + access_token };
var url          = "https://{request.url.hostname}" +
                   "{@uri path="{path|s}"/}";

console.log("REQUEST: GET "+ url);
request.get( url, {headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```

### Response

The list of organizations is returned as a JSON document within the body of the HTTP response.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "org_id":251,
    "name":"Example Org 1"
  },
  {
    "org_id":319,
    "name":"Example Org 2"
  }
]
```
---

## List Workspaces

With an active [*access_token*](#obtain-an-access-token-) you can submit an HTTP GET request to the path `/v2.0/org/[ORG_ID]/workspaces` to obtain a list of *workspaces* within the specified *organization* that the end-user has access to.  (Where `[ORG_ID]` is a valid [organization identifier](#list-organizations).)

### Request

```javascript
var request      = require("request"); // https://www.npmjs.org/package/request
var access_token = "{access_token}";
var headers      = { Authorization: "Bearer " + access_token };
var url          = "https://{request.url.hostname}" +
                   "{@uri path="{path|s}"/}";

console.log("REQUEST: GET "+ url);
request.get( url, {headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```

### Response

The list of workspaces is returned as a JSON document within the body of the HTTP response.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "workspace_id": 862,
    "name": "My Private Notebooks"
  },
  {
    "workspace_id": 973,
    "name": "Sales Leads"
  },
  {
    "workspace_id": 1143,
    "name": "Project Foo"
  }
]
```

---
## List Notes

With an active [*access_token*](#obtain-an-access-token-) you can submit an HTTP GET request to the path `/v2.0/org/[ORG_ID]/workspace/[WORKSPACE_ID]/notes` to obtain a list of *notes* (including *tasks* and *discussions*)  that the end-user has access to within the specified *workspace*.  (Where `[WORKSPACE_ID]` is a valid [workspace identifier](#list-workspaces) and `[ORG_ID]` is a valid [organization identifier](#list-organizations) for the organization that contains the specified workspace.)

### Request

The request may contain one of several query-string parameters that filter or otherwise determine the list of notes that are returned.  In this case, we'll use `note_type=NOTE` to limit the response to true notes (as opposed to tasks or discussions).

```javascript
var request      = require("request"); // https://www.npmjs.org/package/request
var access_token = "{access_token}";
var headers      = { Authorization: "Bearer " + access_token };
var url          = "https://{request.url.hostname}" +
                   "{@uri path="{path|s}"/}";

console.log("REQUEST: GET "+ url);
request.get( url, {headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```

### Response

The list of notes is returned as a JSON document within the body of the HTTP response.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "note_id": 2248,
    "note_type": "NOTE",
    "title": "Lorem Ipsum",
    "body": "Vestibulum convallis a semper...dui euismod elit.",
    "state": "ACTIVE",
    "creator": {
      "user_id": "user4321",
      "given_name": "Jane",
      "family_name": "Doe"
    },
    "tags": [
      {
        "tag_id": 127,
        "name": "Work"
      },
      {
        "tag_id": 564,
        "name": "SampleTag"
      }
    ]
  },
  {
    "note_id": 412,
    "note_type": "NOTE",
    "title": "Accumsan Nisl",
    "body": "Cum sociis natoque penatibus et magnis dis parturient nascetur ridiculus mus.",
    "state": "ACTIVE",
    "creator": {
      "user_id": "user525",
      "given_name": "Finn",
      "family_name": "Sevastyan"
    },
    "tags": [
      {
        "tag_id": 564,
        "name": "to-review"
      }
    ]
  }
]
```

---
## List Incomplete Tasks Assigned to a User

With an active [*access_token*](#obtain-an-access-token-) you can submit an HTTP GET request to the path `/v2.0/org/[ORG_ID]/workspace/[WORKSPACE_ID]/notes` to obtain a list of *notes* (including *tasks* and *discussions*)  that the end-user has access to within the specified *workspace*.  (Where `[WORKSPACE_ID]` is a valid [workspace identifier](#list-workspaces) and `[ORG_ID]` is a valid [organization identifier](#list-organizations) for the organization that contains the specified workspace.)

### Request

The request may contain one of several query-string parameters that filter or otherwise determine the list of notes that are returned.  In this case, we'll use:

  * `note_type=TASK` to limit the response to tasks (as opposed to notes or discussions),
  * `complete=false` to limit the response to tasks that haven't been completed yet, and
  * `assignee==user525` to limit the response to the tasks assigned to a specific user (where `user525` is this user's `user_id` value).

(See the [API documentation](/api/v2.0/#!/notes/get_org_org_id_workspace_workspace_id_notes) for a complete list of and additional information on the supported parameters.)

### Request

```javascript
var request      = require("request"); // https://www.npmjs.org/package/request
var access_token = "{access_token}";
var headers      = { Authorization: "Bearer " + access_token };
var url          = "https://{request.url.hostname}" +
                   "{@uri path="{path|s}"/}";

console.log("REQUEST: GET "+ url);
request.get( url, {headers:headers,json:true}, function(err,response,body) {
  if(err) {
    console.error("Encountered an error",err);
    process.exit(2);
  } else {
    console.log("RESPONSE: Status "+ response.statusCode);
    if(body) {
      console.log(JSON.stringify(body,null,2));
    }
  }
});
```
{@markdown}

### Response

The list of tasks is returned as a JSON document within the body of the HTTP response.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "note_id": 1376,
    "note_type": "TASK",
    "title": "Review Contract",
    "body": "Finn, please review the latest draft of this contract.",
    "state": "ACTIVE",
    "creator": {
      "user_id": "user4321",
      "given_name": "Jane",
      "family_name": "Doe"
    },
    "assignee": {
      "user_id": "user525",
      "given_name": "Finn",
      "family_name": "Sevastyan"
    },
    "tags": [
      {
        "tag_id": 564,
        "name": "to-review"
      }
    ],
    "due":"2013-11-19T06:09:07.435Z"
  },
  {
    "note_id": 8675,
    "note_type": "TASK",
    "title": "Taskum Lorem",
    "body": "Mauris ac <strong>felis vel velit</strong> tristique imperdiet.",
    "state": "ACTIVE",
    "creator": {
      "user_id": "user525",
      "given_name": "Finn",
      "family_name": "Sevastyan"
    },
    "assignee": {
      "user_id": "user525",
      "given_name": "Finn",
      "family_name": "Sevastyan"
    }
  }
]
```
