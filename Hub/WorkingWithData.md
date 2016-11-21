# Working With Data

## Refreshing Data

### Contextual and Notifications

When Hub sends your application a request for data in contextual and notifications, there is an option you return called "refreshAt" (See section 4). This property will tel Hub when to next fetch data from your application in order to refresh the contextual panel for the user and the notifications count. You can also use push notifications.

### MicroApp

You have complete control of when the iframe for your micro app refreshes. Then update the data when new data becomes available. It is standard practice to do this with websockets.

## More Buttons

If you are providing more than thirty results to a user, your data must be paged. This can take place in the form of a more button that the user clicks or from a scrolling action of the user when they scroll to the bottom of the available results.

## Sub-Filters for MicroApp Data

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image11.png)

If you wish to filter your data into multiple views you will need to have sub-filters in your micro app. The current filter should be highlighted and clicking on other tabs should filter the view.
