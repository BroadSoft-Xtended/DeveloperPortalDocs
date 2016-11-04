# Updating Notifications

## How to Update the Notifications Count:

When the user opens the application, we will send you the following request:

```
POST:

URL: https://<yourBaseUrl>/v1/:app/:username/notifications <private API>

Post Body:

{

  auth: <any> (optional)

}
```

OR

URL: https://<baseUrl>/v1/:app/notifications <public API>

This request will hit your API and you will respond with:

```
{

  count: <integer>,

  refreshAt: <Time in milliseconds since January 1 1970 UTC IE: 1463661591313> (optional)

}
```

If your application was registered as public, all optional parameters are not needed or will not be sent.

The **count** is the count to be displayed to the user in the Hub notifications panel.

The **refreshAt** value can be used for simulation push notifications is your application does not support such a feature. It is the next datetime at which Hub will request notifications from your application. This time is in the form of number of milliseconds since January 1st 1970 UTC.

Example in Javascript:

```
var refreshAt = new Date().getTime();

//refreshAt = 1463661591313
```
