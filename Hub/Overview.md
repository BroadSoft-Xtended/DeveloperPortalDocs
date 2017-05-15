# Overview

The Hub API allows developers to use the Hub framework to write apps for Broadsoft customers that integrate with all Broadsoft products.  This document outlines how developers can use the API to deliver a Hub application that will enhance the overall experience of the BroadSoft client experience.

## Intended Audience

This document is intended to help developers build and maintain third party Hub integrations. Technical details will be provided that will aid you in calling the Hub API as well as some best practices.

Hub is a part of several products at Broadsoft. (Team-One, CC-One, UC-One) Hub provides a way for developers to build an app that serves up data to the user that enhances an interaction with another user.

To do this, Hub apps have three different areas where data can be delivered: the MicroApp panel, the Notification bar, and the Contextual panel.   

Below is an illustration of how Hub is displayed in Team-One. Anyone can get a free trial Team-One account. Email us at jodonnell@broadsoft.com if you need your free trial extended.

## Notifications

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image2.png)

In the notifications bar, you’ll have an icon for your application. This icon is also used for navigation to your app.

### What can I set here?

* App icon for notification uploaded on app registration page (SVG, WOFF)

## Contextual

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image3.png)

This panel is shown when a user interacts with another user through chat or a phone call. When this happens, all user enabled apps are sent a request for all user data that they want to display in the contextual panel. This is usually data that is relevant to the users in the conversation. Contextual data is pulled in for all apps and ordered my most recent.

## MicroApp

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image4.png)

The micro app an iframe that you control and host on your servers. The default size of this iframe is 288px by 470px. Users have the ability to expand the window to any height while maintaining a fixed width of 288px. The url for the MicroApp iframe is the base url of your app that you specify in the developer portal.

## User Actions

When users take actions in Hub, your app will be notified and it is up to you to provide data for the user. This may be when a user searches in Hub or clicks on certain buttons in the Hub interface.

## Settings

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image5.png)

When the user first logs into the Hub application, they will see a settings page for all applications. This is where your app will be shown to the users for the first time. The user will attempt to enable your application and then you can start to provide data to that user.

# Application Development Process

## Make it

You will need to create a web application that can be displayed in an iframe. Your application will also need to send REST requests to talk to the API.

## Host it

You can host your application on any hosting service that you wish. The only thing to consider here is that Hub will have to be able to show this publicly hosted service in an iframe. If you are protecting against CORS, you will also need to add the Hub staging urls and productions urls to your whitelist.

In the case where you are not sure how to host an application on the internet, you can follow the following steps as a guide for hosting on Heroku.

## App Registration

Once you are pleased with your application and it has been fully tested, you can register your application here: https://developer.broadsoftlabs.com/#/app/make. After you register your application, you can go to user settings to see your login credentials: https://developer.broadsoftlabs.com/#/app/settings. Your app is now hosted on Hub staging.

## Schedule a Production Release

We will be looking for bugs, UI defects, UI general styling and security vulnerabilities. If your application fails any of these steps, you will be contacted by a Hub team member to aid you in fixing these issues before the application can be enabled on production. Click on your app in the make section and then select 'Submit to App directory'. https://developer.broadsoftlabs.com/#/app/appDashboard/hub/submitToDirectory

# App Registration

## Register Your App

Before you can update any data within Hub, you must register your application. To do this go here https://developer.broadsoftlabs.com then log in and create apps on the make page.

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
