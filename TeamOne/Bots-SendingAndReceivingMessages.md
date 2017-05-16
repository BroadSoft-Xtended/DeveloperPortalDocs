# Getting Started with Bots

## Step 2 - Send and receive RTM API messages.

To use the RTM API your application will send and receive JSON documents over this websocket connection.

# Chat Events

## Sending Chat Messages


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

If an error occurs, you'll get back an error response instead.

## Receiving Chat Messages

By default, your client will be notified (receive a payload) whenever a chat message is added, changed or deleted within one of the workspaces you have access to.

## Chat Message Added

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

## Chat Message Changed

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
  * `type`, `user`, `screen_name`, `given_name`, `family_name`, `workspace_id`, `workspace_name`, `workspace_1on1`, `org_id`, `org_name`, `text`, `at_me`, `at_us`, `from_me`, `note_id`, `channel` and `ts` play the same role as they did in the "chat message added" case.
  * `subtype` is `"message_changed"`, indicating that this chat message was changed.
  * `hidden` is `true`, as it is for all `message_changed` and `message_deleted` payloads. If you are only interested in new messages, you might ignore all payloads for which `hidden` is `true`.
  * `message` contains a description of message _after_ the change was applied.
  * `previous_message` contains a description of the chat message just _before_ the change was applied.

## Chat Message Deleted

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
  * `type`, `user`, `screen_name`, `given_name`, `family_name`, `workspace_id`, `workspace_name`, `workspace_1on1`, `org_id`, `org_name`, `text`, `at_me`, `at_us`, `from_me`, `note_id`, `channel` and `ts` play the same role as they did in the "chat message added" case.
  * `subtype` is `message_deleted`, indicating that this chat message was deleted.
  * `hidden` is `true`, as it is for the `message_changed` case.
  * `previous_message` contains a description of the chat message just before it was removed.

# Typing Events

## Sending Typing Events

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

## Receiving Typing Events

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

# Note Events

## Sending Notes

To create, modify or delete a note (of any type&mdash;including NOTE, TASK, RESOURCE (file), etc.), you may use REST API tunneling or interact with the REST API directly.  There is no note equivalent of the `message` request payload.

## Receiving Notes

When a "note"  (of any type&mdash;including NOTE, TASK, RESOURCE (file), etc.), that the user can access is created, modified or deleted a `note` event is transmitted.

Examples of these events are listed below.

> NOTE: *When a note is moved from one workspace to another, this information is transmitted as _two_ events -- one deleting the note from the old workspace and a second adding the note to the new workspace.  This implies that (depending upon your access to the workspaces), a moved note may look like just a delete, just an add or a delete/add pair.*

> NOTE: *Within Team-One, chat messages are a type of note (literally, `note_type=CHAT`). Hence currently when a chat message is added (or changed or removed) _two_ messages are sent: one `type:message` and another `type:note`.*

> NOTE: *Currently "draft"-status notes are not published via the RTM or webhook interface.  Creating, editing or deleting a draft note is invisible to clients. When a note changes from draft to published, it appears as a "note_added" event to clients. When a note changes from published to draft (private) it appears as a "note_deleted" event to clients (even those associated with the owner of the now-private note).(However, the [REST-tunneling methods](#rest-api-tunneling) will report on and modify draft notes if the proper parameters are set.*

## Note Added

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

## Note Changed

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
 * `event_uuid`, `org_id`, `workspace_id`, `note_id` and `note_type` play the same role as in the `note_added` case.
 * `note` describes the current version of the note (just _after_ the change was applied).
 * `previous_note` describes the previous version of the note (just _before_ the change was applied).

 Again, for Slack compatibility, the following attributes are also included:

  * `channel`, containing `<org-id>/<workspace-id>`
  * `type`, which will be `note`
  * `subtype`, which will be `note_changed`
  * `hidden`, which will be `true`

## Note Deleted

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
 * `event_uuid`, `org_id`, `workspace_id`, `note_id` and `note_type` play the same role as in the `note_added` case.
 * `previous_note` describes the previous version of the note (just before the note was removed).

 Again, for Slack compatibility, the following attributes are also included:

  * `channel`, containing `<org-id>/<workspace-id>`
  * `type`, which will be `note`
  * `subtype`, which will be `note_deleted`
  * `hidden`, which will be `true`

# REST API Tunnelling

You may submit arbitrary requests to the Team-One REST API via the websocket connection. You can test this out by using the swagger API endpoint here: https://developer.broadsoftlabs.com/#/app/swagger.

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

## Ping/Pong

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

# Error Handling

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
