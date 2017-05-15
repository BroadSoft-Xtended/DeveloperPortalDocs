# Push Notifications

This is a request that your app sends to Hub. A push notification is when you send a request to Hub that tell Hub that you have data or a notification to display to a Hub user. Once you send this request, Hub will behave just as if you the user had turned on Hub for the day. This means that hub will request the latest contextual and notification data by sending POST requests to `https://<baseUrl>/:app/notifications and https://<yourBaseUrl>/:app/timeline`.

## Example

1. The user turns on your app in Hub
- Hub calls your notification route hosted on your server
2. The user checks contextual with another user
- Hub calls your contextual route hosted on your server
3. You receive new information you want to show the user at a time when Hub is not updating data from your app
- You call the push notification route and Hub will request both contextual and notifications from your app
4. The hub app returns the updated contextual data and the new notifications count to the user in the Hub UI


```
POST

URL: https://<hubBaseUrl>/:appName/:username/push
```
