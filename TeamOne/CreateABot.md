# How to use the Team-One RTM API

<div style="border-bottom:1px solid #ddd; margin-top:-0.4em; margin-bottom:1em; padding-bottom: 1.4em">
  <div style="float:right;text-align:right;font-size:80%;max-width:33%;">
  Revision 1.2.1<br>Last modified: 4 October 2016</div>
  &nbsp;
  <div style="font-size:60%; float:left; max-width:66%;"><i>This document is proprietary and confidential. No part of this document may be disclosed in any manner to a third party without prior written consent from BroadSoft.</i>
  </div>
</div>

  - [Step 0. Obtain an access token.](#step-0-obtain-an-access-token)
  - [Step 1. Connect to the RTM API.](#step-1-connect-to-the-rtm-api)
    - [Event Filtering Parameters](#event-filtering-parameters)
  - [Step 2. Send and receive RTM API messages.](#step-2-send-and-receive-rtm-api-messages)
      - [Chat Events](#chat-events)
        - [Sending Chat Messages](#sending-chat-messages)
        - [Receiving Chat Messages](#receiving-chat-messages)
          - [Chat Message Added](#chat-message-added)
          - [Chat Message Changed](#chat-message-changed)
          - [Chat Message Deleted](#chat-message-deleted)
      - [Typing Events](#typing-events)
        - [Sending Typing Events](#sending-typing-events)
        - [Receiving Typing Events](#receiving-typing-events)
      - [Note Events](#note-events)
        - [Sending Notes](#sending-notes)
        - [Receiving Notes](#receiving-notes)
          - [Note Added](#note-added)
          - [Note Changed](#note-changed)
          - [Note Deleted](#note-deleted)
      - [REST API Tunneling](#rest-api-tunneling)
      - [Ping/Pong](#pingpong)
      - [Error Handling](#error-handling)
  - [Step 3. Disconnect](#step-3-disconnect)

## Step 0. Obtain an access token.

In order to use the RTM API you will need a Team-One API "access token".

This token allows your application to access Team-One on behalf of a specific end-user.

The Team-One API access token is literally an [OAuth2](https://tools.ietf.org/html/rfc6749) access token.

You can create and manage access tokens for your _own_ account at <https://app.intellinote.net/rest/account/api-tokens>.

To obtain an access token for an arbitrary user's account, you may use the OAuth2 protocol, as described [here](https://app.intellinote.net/rest/content/examples#obtain-an-access-token-and-refresh-token-) and elsewhere.

(Until the access token expires or is revoked by the end-user) this is a one-time action.  The access token will allow your application to access the end-user's account across multiple sessions.

## Step 1. Connect to the RTM API.

The Real-Time-Messaging API is a [websocket](https://en.wikipedia.org/wiki/WebSocket)-based API.

You will establish a single websocket connection to the Team-One RTM service for each application "session".  Within the duration of a single session you may exchange multiple payloads with the RTM service. (Note that unlike the HTTP protocol, in which the conversation is composed of request/response pairs, your RTM client may receive payloads without initiating a request and might submit multiple payloads without expecting a response.)

In order to establish a `wss://`-protocol connection to the RTM API, you must submit an authenticated request to the Team-One REST API. The REST server's response will contain the URL your application can use to establish a websocket connection.

1. Invoke the [`GET /rtms/start`](https://app.intellinote.net/rest/api/v2/#!/rtms/get_rtms_start) REST endpoint by visiting:
   ```
   https://app.intellinote.net/rest/v2/rtms/start
   ```
   This request must include an `Authorization` header containing your access token (or otherwise authenticate to the API).  

   For example, you might submit the request:

   ```http
   GET /rest/v2/rtms/start HTTP/1.1
   Host: app.intellinote.net
   Authorization: Bearer THEACCESSTOKEN
   ```

2. The response will be a JSON document that contains an attribute named `href`. The value will look something like:

   ```
   wss://rtm.intellinote.net/rtms/start?t=...{long-base64-string}....
   ```

   For example, in response to your request above, the server might reply with:

    ```http
    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 118

    { "href": "wss://rtm.intellinote.net/rtms/start?t=VGhpcyBpcyBhbiBleGFtcGxlIG9mIGzZTY0IGVuY29kZWQgc3RyaW5nLg==" }
    ```
   Visit that URL to establish your RTM API socket connection.

3. Once the connection is established you will receive a welcome payload over the websocket connection. The payload will be a JSON document that looks something like this:

   ```json
   { "type" : "hello" }
   ```

   This payload indicates that you have successfully connected to the RTM API. You may now use that connection to send payloads to or receive payloads from the RTM service.

### Event Filtering Parameters

As described in the [REST documentation](https://app.intellinote.net/rest/api/v2/#!/rtms/get_rtms_start), the `GET /rtms/start` accepts several optional query-string parameters that control which events your client will be notified about.

If you need more sophisticated or specialized filtering, note that you can always subscribe to a broader class of events and perform additional filtering within your client.

#### General Parameters

 * The `org_id` and `workspace_id` parameters may contain a comma-delimited list of one or more identifiers. When present the only [chat](#chat-events),  [note](#note-events) or [typing](#typing-events) events your client will receive are those within one of the specified organizations and/or workspaces.
 &nbsp;
 * The `event_type` parameter may contain a comma-delimited list of one or more event type/subtype values from the set { `message_added`, `message_changed`, `message_deleted`, `note_added`, `note_changed`, `note_deleted` }.  When present, the only [chat](#chat-events) or [note](#note-events) events your client will be notified of are those of the specified types. (The special values `message` and `note` can be used as a shorthand way to indicate all three `_added`, `_changed` and `_deleted` types.)
&nbsp;
 * The `matching` parameter may contain a simple string or a regular expression. When present, your client will only be notified of [note events](#note-events) for which the `body` attribute contains the string or matches the pattern and will only be notified of [chat events](#chat-events) for which the `text` attribute contains the string or matches the pattern.  

   Any value that starts and ends with a `/` character (with an optional `i` suffix following the ending `/` to indicate a case-insensitive match) such as `/^foo\s*bar/` or `/foo/i` will be interpreted as a [regular-expression](https://en.wikipedia.org/wiki/Regular_expression).

 #### Note-Event Specific Parameters

 When a note-specific filtering parameter is used, your client will not be notified of any [chat events](#chat-events).

  * The `note_type` parameter may contain a single or comma-delimited list of note types (`NOTE`, `TASK`, `CHAT`, `RESOURCE`, etc.). When present your  client will only be notified of [note events](#note-events) for which the `note_type` attribute of one or both of the "current" and "previous" note objects matches an element of the list.

 Note-specific and chat-specific parameters cannot be used at the same time.

#### Chat-Event Specific Parameters

When a chat-specific filtering parameter is used, your client will not be notified of any [note events](#note-events).
&nbsp;
  * The `from_me` parameter is a boolean-valued (true/false) flag.
    * When `true`, your client will only be notified of [chat events](#chat-events) that were created by the end-user associated with your client.
    * When `false`, your client will only be notified of [chat events](#chat-events) that were _NOT_ created by the associated end-user.
&nbsp;
* The `at_me` and `at_us` parameters are also boolean-valued (true/false) flags.

  * These parameters filter based on:
    * whether the chat message is specifically targeted at the end-user associated with your client (`at_me`; typically by either (a) including the end-user's "screen name" in an @mention token or (b) appearing in a one-on-one chat with the user), and
    * whether the chat message is targeted at a _group_ of users that includes the end-user associated with your client (`at_us`; typically by the presence of an `@all` or similar token.
&nbsp;
  * When only one of `at_me` or `at_us` is specified, your client will be only notified of chat events that match the specified criterion. In other words:
    * When `at_me` is `true`, your client will only be notified of chat events that are specifically directed at the end-user. (*`me`*)
    * When `at_me` is `false`, your client will only be notified of chat events that are _NOT_ specifically directed at the end-user.  (*`NOT(me)`*)
    * And similarly for `at_us`.  (*`us`* and *`NOT(us)`*)
&nbsp;
  * When _both_ `at_me` _and_ `at_us` are specified, the precise behavior depends on the specific combination of values:
     * When both are `true`, your client will only be notified of chat events that are directed at the end-user _or_ at a group that includes the end user. (*`me OR us`*)
     * When both are `false`, your client will only be notified of chat events that are _neither_ directed at the end-user _nor_ at a group that includes the end user. (*`NOT(me OR us)`*)
     * When `at_me` is `true` and `at_us` is `false` your client will only be notified of chat events that are directed at the end-user _and not_ directed at a group that includes the end user. (*`me AND NOT(us)`*)
     * When `at_me` is `false` and `at_us` is `true` your client will only be notified of chat events that are directed  at a group that includes the end user _AND NOT_ directed at the end-user specifically. (*`us AND NOT(me)`*)
&nbsp;
* The `or_matching` parameter is identical to the general `matching` parameter, save that it is an alternative to (ORed with) the `at_me` and `at_us` parameters, rather than ANDed with those parameters like `matching` is.
  &nbsp;
  Hence a filter combination such as:

      at_me=true&or_matching=/^\/foobar/

  can be used to select chat events which are directly targeted at the end-user *or* that start with "slash-command" `/foobar`, while:

        at_me=true&matching=/^\/foobar/

  requires that chat message is _both_ targeted at the end-user *and* start with `/foobar`.

Note-specific and chat-specific parameters cannot be used at the same time.


## Step 2. Send and receive RTM API messages.

To use the RTM API your application will send and receive JSON documents over this websocket connection.

#### Chat Events

##### Sending Chat Messages


To post a chat message to a workspace (or one-on-one chat), you may submit a payload like the following:

```json
{
  "type"         : "message",
  "text"         : "Hello world",
  "org_id"       : 12345,
  "workspace_id" : 5678,
  "id"           : 9933
}
```

where:

 * `type` is `"message"`
 * `text` is the body of the message
 * `org_id` and `workspace_id` indicate where the message should be posted
 * `id` is an optional, arbitrary value that will be echoed in the response. You may use this client-specified `id` value to connect the status response (or error message) to the original request.

For compatibility with the popular messaging service Slack, you may use this alternative form:

```json
{
  "type"         : "message",
  "text"         : "Hello world",
  "channel"      : "12345/5678",
  "id"           : 9933
}
```
where `channel` is simply `<org-id>/<workspace-id>`.

Once your payload has been received by the service, you will receive a response like this:

```json
{
  "ok"       : true,
  "reply_to" : 9933,
  "text"     : "Hello world",
  "note_id"  : 456789,
  "ts"       : "456789"
}
```

where

 * `ok` is `true`
 * `reply_to` is the `id` that was provided in the original `message` payload
 * `text` is the text of the message (which may be different due to URL-detection and similar logic.)
 * `note_id` is the unique identifier assigned to the published chat message

For compatibility with Slack, the following redundant attribute is also included:

 * `ts` is the `note_id` value (as a string).

If an error occurs, you'll get back an [error response](#error-handling) instead.

##### Receiving Chat Messages

By default, your client will be notified (receive a payload) whenever a chat message is added, changed or deleted within one of the workspaces you have access to.

###### Chat Message Added

When a chat message is posted to a workspace your client has access to, you'll receive a payload like the following:

```json
{
  "type"                : "message",
  "subtype"             : "message_added",
  "user"                : "user1234",
  "screen_name"         : "jdoe1234",
  "given_name"          : "Jane",
  "family_name"         : "Doe",
  "workspace_id"        : 5678,
  "workspace_name"      : "Project Alpha",
  "workspace_1on1"      : false,
  "org_id"              : 12345,
  "org_name"            : "Acme Corp",
  "text"                : "Hello World",
  "at_me"               : false,
  "at_us"               : false,
  "from_me"             : false,
  "note_id"             : 456789,
  "channel"             : "12345/5678",
  "ts"                  : "456789"
}
```

where:

  * `type` is `"message"`
  * `subtype` is `"message_added"`
  * `user` is the user identifier (`user_id`) of the user that submitted the chat message
  * `screen_name` is the "handle" of the user that submitted the chat message, which is useful for `@mention` tags.
  * `given_name` is the "first name" of the user that submitted the chat message.
  * `family_name` is the "last name" of the user that submitted the chat message.
  * `workspace_id` and `workspace_name` describe the workspace (or one-on-one chat) in which the message was posted.
  * `workspace_1on1` is `true` if this is a special one-on-one "chat" workspace. It may be `false`, `null` or undefined otherwise. Messages sent in a one-on-one workspace may be treated as if they have an implicit `@mention` tag.
  * `org_id` and `org_name` describe the organization in which this message was posted.
  * `text` is the actual content of the chat message.
  * `at_me` will be `true` if this message is directed at the user that the client represents, either by an explicit `@screen_name` mention or by occurring within a one-on-one workspace. It may be `false`, `null` or undefined otherwise.
  * `at_us` will be `true` if this message is directed at a group that happens to include that user. typically as part of an `@all` or similar mention. It may be `false`, `null` or undefined otherwise.
  * `from_me` will be `true` if this message was added by the user that this client represents. It may be `false`, `null` or undefined otherwise.
  * `note_id` is the unique identifier for the chat message (which within Team-One, is a special type of "note".)

For compatibility with the popular messaging service Slack, the following redundant attributes are also included:

  * `channel` contains `<org-id>/<workspace-id>` and uniquely identifies the workspace (or one-on-one chat) in which the message was posted.
  * `ts` contains `<note-id>` and uniquely identifies this specific chat message.

###### Chat Message Changed

When a chat message in a workspace your client has access to is edited, you'll receive a payload like the following:

```json
{
  "type"                : "message",
  "subtype"             : "message_changed",
  "user"                : "user1234",
  "screen_name"         : "jdoe1234",
  "given_name"          : "Jane",
  "family_name"         : "Doe",
  "workspace_id"        : 5678,
  "workspace_name"      : "Project Alpha",
  "workspace_1on1"      : false,
  "org_id"              : 12345,
  "org_name"            : "Acme Corp",
  "note_id"             : 456789,
  "message"             : {
    "type"                : "message",
    "user"                : "user1234",
    "screen_name"         : "jdoe1234",
    "given_name"          : "Jane",
    "family_name"         : "Doe",
    "workspace_id"        : 5678,
    "workspace_name"      : "Project Alpha",
    "workspace_1on1"      : false,
    "org_id"              : 12345,
    "org_name"            : "Acme Corp",
    "text"                : "@world Hello",
    "at_me"               : false,
    "at_us"               : true,
    "from_me"             : false,
    "note_id"             : 456789,
    "channel"             : "12345/5678",
    "ts"                  : "456789"
  },
  "previous_message"    : {
    "type"                : "message",
    "user"                : "user1234",
    "screen_name"         : "jdoe1234",
    "given_name"          : "Jane",
    "family_name"         : "Doe",
    "workspace_id"        : 5678,
    "workspace_name"      : "Project Alpha",
    "workspace_1on1"      : false,
    "org_id"              : 12345,
    "org_name"            : "Acme Corp",
    "text"                : "Hello World",
    "at_me"               : false,
    "at_us"               : false,
    "from_me"             : false,
    "note_id"             : 456789,
    "channel"             : "12345/5678",
    "ts"                  : "456789"
  },
  "channel"             : "12345/5678",
  "ts"                  : "456789",
  "hidden"              : true  
}
```

Where
  * `type`, `user`, `screen_name`, `given_name`, `family_name`, `workspace_id`, `workspace_name`, `workspace_1on1`, `org_id`, `org_name`, `text`, `at_me`, `at_us`, `from_me`, `note_id`, `channel` and `ts` play the same role as they did in the ["chat message added"](#chat-message-added) case.
  * `subtype` is `"message_changed"`, indicating that this chat message was changed.
  * `hidden` is `true`, as it is for all `message_changed` and `message_deleted` payloads. If you are only interested in new messages, you might ignore all payloads for which `hidden` is `true`.
  * `message` contains a description of message _after_ the change was applied.
  * `previous_message` contains a description of the chat message just _before_ the change was applied.

###### Chat Message Deleted

When a chat message is removed from a workspace your client has access to, you'll receive a payload like the following:

```json
{
  "type"             : "message",
  "subtype"          : "message_deleted",
  "user"             : "user1234",
  "screen_name"      : "jdoe1234",
  "given_name"       : "Jane",
  "family_name"      : "Doe",
  "workspace_id"     : 5678,
  "workspace_name"   : "Project Alpha",
  "workspace_1on1"   : false,
  "org_id"           : 12345,
  "org_name"         : "Acme Corp",
  "note_id"          : 456789,
  "previous_message" : {
    "type"                : "message",
    "user"                : "user1234",
    "screen_name"         : "jdoe1234",
    "given_name"          : "Jane",
    "family_name"         : "Doe",
    "workspace_id"        : 5678,
    "workspace_name"      : "Project Alpha",
    "workspace_1on1"      : false,
    "org_id"              : 12345,
    "org_name"            : "Acme Corp",
    "text"                : "@world Hello",
    "at_me"               : false,
    "at_us"               : true,
    "from_you"            : false,
    "note_id"             : 456789,
    "channel"             : "12345/5678",
    "ts"                  : "456789"
  },
  "channel"          : "12345/5678",
  "ts"               : "456789",
  "hidden"           : true
}
```

Where
  * `type`, `user`, `screen_name`, `given_name`, `family_name`, `workspace_id`, `workspace_name`, `workspace_1on1`, `org_id`, `org_name`, `text`, `at_me`, `at_us`, `from_me`, `note_id`, `channel` and `ts` play the same role as they did in the ["chat message added"](#chat-message-added) case.
  * `subtype` is `message_deleted`, indicating that this chat message was deleted.
  * `hidden` is `true`, as it is for the `message_changed` case.
  * `previous_message` contains a description of the chat message just before it was removed.

#### Typing Events

##### Sending Typing Events

To communicate that the user is currently typing, you may send a payload like this:

```json
{
 "type"         : "typing",
 "org_id"       : 12345,
 "workspace_id" : 5678
}
```

where `org_id` and `workspace_id` indicate conversation into which the user is typing. (The account you used to authenticate indicates _which_ user is typing.)

Or, for compatibility with Slack, you may use this alternative form:

```json
{
 "type"    : "typing",
 "channel" : "12345/5678"
}
```

where `channel` is simply `<org-id>/<workspace-id>`.

##### Receiving Typing Events

When *another* user is typing, you'll receive a payload like this:

```json
{
 "type"         : "user_typing",
 "org_id"       : 12345,
 "workspace_id" : 5678,
 "user"         : "user1234",
 "channel"      : "12345/5678"
}
```

Where:

* `type` is `user_typing`
* `user` is the `user_id` of the user that is typing, and
* `org_id` and `workspace_id` indicate the conversation into which the user is typing

For compatibility with Slack, once again the redundant attribute `channel` is sent, containing `<org-id>/<workspace-id>`.

#### Note Events

##### Sending Notes

To create, modify or delete a note (of any type&mdash;including NOTE, TASK, RESOURCE (file), etc.), you may use [REST API tunneling](#rest-api-tunneling) or interact with the REST API directly.  There is no note equivalent of the `message` request payload.

##### Receiving Notes

When a "note"  (of any type&mdash;including NOTE, TASK, RESOURCE (file), etc.), that the user can access is created, modified or deleted a `note` event is transmitted.

Examples of these events are listed below.

> NOTE: *When a note is moved from one workspace to another, this information is transmitted as _two_ events -- one deleting the note from the old workspace and a second adding the note to the new workspace.  This implies that (depending upon your access to the workspaces), a moved note may look like just a delete, just an add or a delete/add pair.*

> NOTE: *Within Team-One, chat messages are a type of note (literally, `note_type=CHAT`). Hence currently when a chat message is added (or changed or removed) _two_ messages are sent: one `type:message` and another `type:note`.*

> NOTE: *Currently "draft"-status notes are not published via the RTM or webhook interface.  Creating, editing or deleting a draft note is invisible to clients. When a note changes from draft to published, it appears as a "note_added" event to clients. When a note changes from published to draft (private) it appears as a "note_deleted" event to clients (even those associated with the owner of the now-private note).(However, the [REST-tunneling methods](#rest-api-tunneling) will report on and modify draft notes if the proper parameters are set.*

###### Note Added

When a note (of any type) is added to a workspace the client has access to, you'll receive a payload like the following:

```json
{
  "event_uuid"          : "9240f4007f8f11e68ed37336ad970721",
  "event_type"          : "note_added",
  "org_id"              : 12345,
  "workspace_id"        : 5678,
  "note_id"             : 456789,
  "note_type"           : "TASK",
  "note"                : {
    "note_id"             : 456790,
    "note_uuid"           : "3b7d650f41164fbfb784e602a69a7539",
    "note_type"           : "TASK",
    "title"               : "New task for Jack",
    "body"                : "Lorem ipsum...",
    "workspace"           : {
      "workspace_id"        : 5678,
      "name"                : "Project Alpha",
      "1on1"                : false,
      "org"                 : {
        "org_id"              : 12345,
        "name"                : "Acme Corp"
      }  
    },  
    "creator"             : {
      "user_id"             : "user1234",
      "given_name"          : "Jane",
      "family_name"         : "Doe",
      "email"               : "jdoe@example.com",
      "screen_name"         : "jdoe1234"
    },  
    "draft"               : false,
    "created"             : "2016-09-17T15:44:44.878Z",
    "modified"            : "2016-09-17T15:44:44.878Z",
    "assignee"            : {
      "user_id"             : "user5678",
      "given_name"          : "Jack",
      "family_name"         : "Smith",
      "email"               : "jsmith@acme.net",
      "screen_name"         : "jsmith5678"
    },    
    "due"                   : "2016-09-17T17:51:08.821Z",
    "complete"              : false,
  },
  "type"               : "note",
  "subtype"            : "note_added",
  "channel"            : "12345/12345"
}  
```
Where:

 * `event_uuid` is a unique identifier for this payload
 * `event_type` is `"note_added"`, indicating that a note was added.
 * `workspace_id` and `workspace_name` describe the workspace (or one-on-one chat) in which the message was posted.
 * `workspace_1on1` is `true` if this is a special one-on-one "chat" workspace. It may be `false`, `null` or undefined otherwise. Messages sent in a one-on-one workspace may be treated as if they have an implicit `@mention` tag.
 * `org_id` and `org_name` describe the organization in which this message was posted.
 * `note_id` is a unique identifier for this note.
 * `note_type` indicates the type of note that was created (typically `NOTE`,`TASK`,`CHAT` or `RESOURCE`).
 * `note` contains a description of the note object itself (which may contain various attributes depending upon the `note_type`).

 For compatibility with Slack, once again the redundant attribute `channel` is sent, containing `<org-id>/<workspace-id>` and a `type` attribute (equal to `note`) is included.

###### Note Changed

When a note (that the client has access to) is _modified_, a `note_changed` event like the following is transmitted:

```json
{
  "event_uuid"       : "9240f4007f8f11e68ed37336ad970721",
  "event_type"       : "note_changed",
  "org_id"           : 12345,
  "workspace_id"     : 5678,
  "note_id"          : 456789,
  "note_type"        : "TASK",
  "note"             : {
    "note_id"          : 456789,
    "note_uuid"        : "3b7d650f41164fbfb784e602a69a7539",
    "note_type"        : "TASK",
    "title"            : "New task for Jack",
    "body"             : "Lorem ipsum...",
    "workspace"        : {
      "workspace_id"     : 5678,
      "name"             : "Project Alpha",
      "1on1"             : false,
      "org"              : {
        "org_id"           : 12345,
        "name"             : "Acme Corp"
      }  
    },  
    "creator"          : {
      "user_id"          : "user1234",
      "given_name"       : "Jane",
      "family_name"      : "Doe",
      "email"            : "jdoe@example.com",
      "screen_name"      : "jdoe1234"

    },  
    "draft"            : false,
    "created"          : "2016-09-17T15:44:44.878Z",
    "modified"         : "2016-09-17T15:44:44.878Z",
    "assignee"         : {
      "user_id"          : "user5678",
      "given_name"       : "Jack",
      "family_name"      : "Smith",
      "email"            : "jsmith@acme.net",
      "screen_name"      : "jsmith5678"
    },  
    "due"              : "2016-09-17T17:51:08.821Z",
    "complete"         : false
  },
  "previous_note"             : {
    "note_id"          : 456789,
    "note_uuid"        : "3b7d650f41164fbfb784e602a69a7539",
    "note_type"        : "TASK",
    "title"            : "New task for Jack",
    "body"             : "Lorem ipsum...",
    "workspace"        : {
      "workspace_id"     : 5678,
      "name"             : "Project Alpha",
      "1on1"             : false,
      "org"              : {
        "org_id"           : 12345,
        "name"             : "Acme Corp"
      }  
    },  
    "creator"          : {
      "user_id"          : "user1234",
      "given_name"       : "Jane",
      "family_name"      : "Doe",
      "email"            : "jdoe@example.com",
      "screen_name"      : "jdoe1234"
    },  
    "draft"            : false,
    "created"          : "2016-09-17T15:44:44.878Z",
    "modified"         : "2016-09-17T15:44:44.878Z",
    "assignee"         : {
      "user_id"          : "user5678",
      "given_name"       : "Jack",
      "family_name"      : "Smith",
      "email"            : "jsmith@acme.net",
      "screen_name"      : "jsmith5678"
    },  
    "due"              : "2016-09-17T17:51:08.821Z",
    "complete"         : false
  },
  "type"             : "note",
  "subtype"          : "note_changed",
  "channel"          : "12345/5678",
  "hidden"           : true
}
```


Where:

 * `event_uuid` is a unique identifier for this payload
 * `event_type` is `"note_changed"`, indicating that a note was modified.
 * `event_uuid`, `org_id`, `workspace_id`, `note_id` and `note_type` play the same role as in the [`note_added`](#note-added) case.
 * `note` describes the current version of the note (just _after_ the change was applied).
 * `previous_note` describes the previous version of the note (just _before_ the change was applied).

 Again, for Slack compatibility, the following attributes are also included:

  * `channel`, containing `<org-id>/<workspace-id>`
  * `type`, which will be `note`
  * `subtype`, which will be `note_changed`
  * `hidden`, which will be `true`

###### Note Deleted

When a note (that the client has access to) is _removed_, a `note_deleted` event like the following is transmitted:

```json
{
  "event_uuid"       : "9240f4007f8f11e68ed37336ad970721",
  "event_type"       : "note_deleted",
  "org_id"           : 12345,
  "workspace_id"     : 5678,
  "note_id"          : 456789,
  "note_type"        : "TASK",
  "previous_note"    : {
    "note_id"          : 456789,
    "note_uuid"        : "3b7d650f41164fbfb784e602a69a7539",
    "note_type"        : "TASK",
    "title"            : "New task for Jack",
    "body"             : "Lorem ipsum...",
    "workspace"        : {
      "workspace_id"     : 5678,
      "name"             : "Project Alpha",
      "1on1"             : false,
      "org"              : {
        "org_id"           : 12345,
        "name"             : "Acme Corp"
      }  
    },  
    "creator"          : {
      "user_id"          : "user1234",
      "given_name"       : "Jane",
      "family_name"      : "Doe",
      "email"            : "jdoe@example.com",
      "screen_name"      : "jdoe1234"
    },  
    "draft"            : false,
    "created"          : "2016-09-17T15:44:44.878Z",
    "modified"         : "2016-09-17T15:44:44.878Z",
    "assignee"         : {
      "user_id"          : "user5678",
      "given_name"       : "Jack",
      "family_name"      : "Smith",
      "email"            : "jsmith@acme.net",
      "screen_name"      : "jsmith1234"
    },  
    "due"              : "2016-09-17T17:51:08.821Z",
    "complete"         : false
  },
  "type"             : "note",
  "subtype"          : "note_changed",
  "channel"          : "12345/5678",
  "hidden"           : true
}
```

Where:

 * `event_uuid` is a unique identifier for this payload
 * `event_type` is `"note_deleted"`, indicating that a note was removed.
 * `event_uuid`, `org_id`, `workspace_id`, `note_id` and `note_type` play the same role as in the [`note_added`](#note-added) case.
 * `previous_note` describes the previous version of the note (just before the note was removed).

 Again, for Slack compatibility, the following attributes are also included:

  * `channel`, containing `<org-id>/<workspace-id>`
  * `type`, which will be `note`
  * `subtype`, which will be `note_deleted`
  * `hidden`, which will be `true`

#### REST API Tunneling

You may submit arbitrary requests to the Team-One REST API via the websocket connection.

To submit a REST API request, send a payload such as:

```json
{
  "type"     : "rest/request",
  "method"   : "GET",
  "path"     : "/user/-",
  "qs"       : { "x":"y" },
  "body"     : { "a":"b" },
  "headers"  : { "X-Header":"Example" },
  "id"       : "7465962651554862bffa9e8f45e4bba7"
}
 ```

 Where:
  * `type` is `rest/request`
  * `method` and `path` are required and specify the REST endpoint to invoke.  Note that path will be appended to the base-url of `https://app.intellinote.net/rest/v2`.
  * `qs`, `body` and `headers` are optional query-string, request body and request header descriptions. `body` is ignored for `GET` and `DELETE` methods.
  * `id` is an optional client-specified identifier for this request. (When present, the value will be included in the `reply_to` attribute of the response.)

When the request is processed you'll get back a response like the following:

```json
{
  "type"     : "rest/response",
  "reply_to" : "7465962651554862bffa9e8f45e4bba7",
  "status"   : 200,
  "body"     : { "user_id":"user1234" },
}
 ```

Where:
  * `type` is `rest/response`
  * `reply_to` echoes the `id` attribute of the request.
  * `status` is the HTTP status code of the response.
  * `body` contains the response body (if any)

#### Ping/Pong

To keep the connection alive or just to test it, you may send a "ping" payload, like this:

```json
{
  "type" : "ping"
}
```

and you'll get back a response like this:

```json
{
  "type"   : "pong"
}
```

Note that any extra attributes included in the `ping` payload will be copied to the `pong` payload, so you can do something like this:

```json
{
  "type" : "ping",
  "sent" : 1474128842745
}
```

and you'll get back a response like this:

```json
{
  "type"   : "pong",
  "sent"   : 1474128842745
}
```

which you could then use to measure the latency between client and server.

Note that a payload comprised of a blank (empty) string will also be interpreted as a `ping` payload.

#### Error Handling

When an error occurs you'll get back a payload like the following:

```json
{
  "ok"     :false,
  "type"   :"error",
  "error"  : {
    "code"   : 401,
    "msg"    : "Not authorized."
  }
}
```

Where:

 * `ok` is `false`
 * `type` is `"error"`
 * `error` describes the error in more detail:
    * `code` is an HTTP-*like* status code
    * `msg` is a human-readable error message

In some cases the `error` payload may contain additional attributes. For example, when an error occurs while attempting to post a new message to a workspace, the `error` response will contain `reply_to`, indicating the `id` value submitted with the original `message` request.

## Step 3. Disconnect

When you are done, you may send a `goodbye` payload in order to gracefully close your session (effectively "logging out" of the RTM API).

Simply submit a JSON document like the following:

```json
{ "type" : "goodbye" }
```

The server will respond with:

```json
{ "type" : "goodbye" }
```

and close the websocket connection.

Note that you can also send the plain-text payload `goodbye` or just `bye` to accomplish this.

This step is not strictly necessary, your session will be closed if your client disconnects or if it remains idle for too long.
