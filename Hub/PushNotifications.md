# Push Notifications

A push notification is when you send a request to Hub that tell Hub that you have data or a notification to display to a Hub user. Once you send this request, Hub will behave just as if you the user had turned on Hub for the day. This means that Hub will call your app for contextual and notifications data.

## Example

1. The user turns on your app in Hub
- Hub calls your notification route hosted on your server
2. The user checks contextual with another user
- Hub calls your contextual route hosted on your server
3. You receive new information you want to show the user at a time when Hub is not updating data from your app
- You call the push notification route and Hub will request both contextual and notifications from your app

```
POST

URL: https://<hubBaseUrl>/:appName/:username/push
```
