# Getting Started with Bots

## Step 1. Connect to the RTM API.

The Real-Time-Messaging API is a [websocket](https://en.wikipedia.org/wiki/WebSocket)-based API.

You will establish a single websocket connection to the Team-One RTM service for each application "session".  Within the duration of a single session you may exchange multiple payloads with the RTM service. (Note that unlike the HTTP protocol, in which the conversation is composed of request/response pairs, your RTM client may receive payloads without initiating a request and might submit multiple payloads without expecting a response.)

In order to establish a `wss://`-protocol connection to the RTM API, you must submit an authenticated request to the Team-One REST API. The REST server's response will contain the URL your application can use to establish a websocket connection.

1. Invoke the [`GET /rtms/start`](https://developer.broadsoftlabs.com/#/app/swagger) REST endpoint by visiting:
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

As described in the [REST documentation](https://developer.broadsoftlabs.com/#/app/swagger), the `GET /rtms/start` accepts several optional query-string parameters that control which events your client will be notified about.

If you need more sophisticated or specialized filtering, note that you can always subscribe to a broader class of events and perform additional filtering within your client.

#### General Parameters

 * The `org_id` and `workspace_id` parameters may contain a comma-delimited list of one or more identifiers. When present the only chat note or typing events your client will receive are those within one of the specified organizations and/or workspaces.
 &nbsp;
 * The `event_type` parameter may contain a comma-delimited list of one or more event type/subtype values from the set { `message_added`, `message_changed`, `message_deleted`, `note_added`, `note_changed`, `note_deleted` }.  When present, the only chat or note events your client will be notified of are those of the specified types. (The special values `message` and `note` can be used as a shorthand way to indicate all three `_added`, `_changed` and `_deleted` types.)
&nbsp;
 * The `matching` parameter may contain a simple string or a regular expression. When present, your client will only be notified of note events for which the `body` attribute contains the string or matches the pattern and will only be notified of chat events for which the `text` attribute contains the string or matches the pattern.  

   Any value that starts and ends with a `/` character (with an optional `i` suffix following the ending `/` to indicate a case-insensitive match) such as `/^foo\s*bar/` or `/foo/i` will be interpreted as a [regular-expression](https://en.wikipedia.org/wiki/Regular_expression).

 #### Note-Event Specific Parameters

 When a note-specific filtering parameter is used, your client will not be notified of any chat events.

  * The `note_type` parameter may contain a single or comma-delimited list of note types (`NOTE`, `TASK`, `CHAT`, `RESOURCE`, etc.). When present your  client will only be notified of note events for which the `note_type` attribute of one or both of the "current" and "previous" note objects matches an element of the list.

 Note-specific and chat-specific parameters cannot be used at the same time.

#### Chat-Event Specific Parameters

When a chat-specific filtering parameter is used, your client will not be notified of any note events.
&nbsp;
  * The `from_me` parameter is a boolean-valued (true/false) flag.
    * When `true`, your client will only be notified of chat events that were created by the end-user associated with your client.
    * When `false`, your client will only be notified of chat events that were _NOT_ created by the associated end-user.
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
