# Getting Started with Bots

## Step 3 - Disconnect

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
