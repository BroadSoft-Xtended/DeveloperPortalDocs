# UC-One Hub API - Working With Data

## Gathering data from users

You may track user statistics from services such as google analytics in any way you see fit for user interactions that pertain to your application within the micro apps iframe.

You may not store any information that relates to user interaction with Hub or Communicator. This includes how the user interacts with notifications, contextual or as they switch between apps in Hub. You must also never track the user by any means that will allow you to profile a user and store user information. All data must be anonymized through one way encryption.

Please see the full list of requirements in the terms and conditions section of this document.

## Refreshing Data

### Contextual and Notifications

When Hub sends your application a request for data in contextual and notifications, there is an option you return called "refreshAt" (See section 4). This property will tel Hub when to next fetch data from your application in order to refresh the contextual panel for the user and the notifications count.

### MicroApp

You have complete control of when the iframe for your micro app refreshes. The best practice is to update the data when new user data becomes available. It is standard practice to do this with websockets. The screen should never flicker with new data and you should never see the page reload. Rather the data should just update as it becomes available to the user.

## More Buttons

If you are providing more than thirty results to a user, your data must be paged. This means that you will only send the first thirty results on first request and as the user request more data, you will serve up the next set of results. This can take place in the form of a more button that the user clicks or from a scrolling action of the user when they scroll to the bottom of the available results.

This is in place as there are several apps loading data and it is better to only serve a small set of results to limit user bandwidth and increase performance while loading the application initially.

## Searching Data

When a user attempts to search your data that you provide in your microapp, you should attempt to provide search results as quickly as possible. Your application should search all front end results initially and filter them on the front end for the user to view. This should be the first  stage of your search. While you are filtering your front end results, you should also search your backend for the data as well and once the results come back from your server, you will need to merge the two searches.

## Sub-Filters for MicroApp Data

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image11.png)

If you wish to filter your data into multiple views you will need to have a sun-filters in your micro app.

The current tab should be highlighted and clicking on other tabs should filter the view. Data for each tab should be cached so that when the user switches tabs, the screen does not flash and a reload is not required.
