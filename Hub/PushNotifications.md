# Push Notifications

This is a request that your app sends to Hub. A push notification is when you send a request to Hub that tell Hub that you have a new notification to display to a Hub user. Once you send this request, Hub will behave just as if you the user had turned on Hub for the day. This means that Hub will request the latest notification count by sending POST requests to `https://<yourAppBaseUrl>/:app/notifications`.

## Example

1. The user turns on your app in Hub
- Hub calls your notification route hosted on your server
2. The user checks contextual with another user
- Hub displays your contextual iframe
3. At a later time, you receive new information you want to show the user at a time when Hub is not updating data from your app
- You call the push notification route and Hub will request notifications from your app
4. The Hub app returns the updated notifications count to the user in the Hub UI


```
POST

URL: https://<theHubUrlOfTheUser>/:username/push
```
