# UC-One Hub API - Overview

UC-One Hub APIs will allow BroadSoft Service Providers and Partners to leverage the Hub development framework to bring the power of cloud applications to their customers.  This document will outline how developers will engage the API to deliver a Hub application to enhance the overall experience of the BroadSoft client experience.  

## Intended Audience

This document is intended to help developers build and maintain third party Hub integrations. Technical details will be provided that will aid you in calling the Hub API as well as best practices for working with Hub.

UC-One Hub is a BroadSoft cloud service providing direct access to cloud applications that have been pre-integrated with UC-One – allowing users to be more efficient and productive. These integrations bring a lite client experience and contextual data to your day-to-day communications via UC-One.  UC-One Communicator for Desktop-Chrome is the first client to be integrated with Hub, with other client experiences to follow.  

To do this, Hub integrates into the Chrome client experience in three different ways, the MicroApp View, the Notification bar (which also serves as the navigation bar), and the Contextual data view.   

As you build your integration into Hub, your application data will be inserted through the use of a "webview" or iframe elements.

Below is an illustration of how the three components of Hub (Notifications, Contextual, MicroApp) integrate into the UC-One Chrome experience.

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image1.png)

## Notifications

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image2.png)

In the notifications bar, you’ll have an icon for your application, which will act as a reference point for application notifications and navigation. This view is always shown while communicator is open and provides users a heads-up view of items that require their action and a point to navigate to different applications.

When a user clicks on the notifications icon that corresponds to your app, the MicroApp pane will then shift to your MicroApp pane regardless of where the user is in Communicator at the time.

What can I set here?

* App icon for notification uploaded on app registration page (SVG, WOFF)

* This is elaborated in section 4.2

## Contextual

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image3.png)

This panel is shown when an end user interacts with another user through a chat or phone call. When this happens, all enabled apps that have the contextual box checked under settings are sent a request for all user data that they want to display in the contextual view. This data can send a maximum of 30 items on first call with additional content made available utilizing the "more" function. The next request to your API will ask for the next page.

You will be able to display data here in a container 228px by 70px in size. Your icon will also be used here. This is the default size of the window but keep in mind that the user can expand this window to any height while leaving the width constant.

## MicroApp

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image4.png)

The micro app is where you can display any content that you would like for your application. You will display this information in the form of an iframe. The url of this iframe can be set when you register your application. It is a scrollable container and the header of this panel will have the name of the application that you registered with Hub. The settings link in the header takes you to the Hub settings page.

The default size of this iframe is 288px by 470px. Users also have the ability to expand the window to any height that their monitor supports while maintaining a fixed width of 288px.

There are a few iframes at work here that you may need to consider. The Communicator application provides Hub with three IFrames so Hub can display its content. As a third party developer, you will only be provided with one IFrame in which to display data (This is the MicroApp IFrame).

All the IFrames in the application talk to each other through two APIs: the internal Hub API and the third party API. Public access is only provided for the third party API.

## BroadSearch

BroadSearch is one of the most important parts of Hub as it allows users to easily search through data between all integrations. These can be internal or external applications and allows Hub to combine data of all types in a global search.

There are two places that the user can use the BroadSearch feature: In the micro apps view and in the contextual view. However, when the user searches in the micro apps view, only the current application is searched. In contextual however, search behaves on a more global level.

In contextual, if you search the while in the "All" tab, you will search all applications at once. As you filter the contextual view to categories or subcategories, you then start to limit the scope of the BroadSearch feature to the desired applications.

## Settings

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image5.png)

When the user first logs into the Hub application, they will see a settings page for all applications.

Here you can see the settings page. This is an example of a user only having the first two integrations enabled.

Once an integration is enabled, the blue slider toggles to "ON" and more details become available. You can then click the “Launch” button to navigate to the integration micro app view.

You can also toggle where you want the application data displayed. By disabling contextual, your data will no longer be available in the contextual view. By disabling notifications, you will disable the notification counts and updates. Disabling this does not remove the application from the notifications panel entirely as it will still be used when a user clicks the application

icon corresponding the affected application and Hub navigates

them to the micro app view for that application.

The user can also view which account is linked to this integration. This is helpful if you want to determine where the data is coming from in the case where a user might have multiple accounts for a single application. A good example of this is gmail personal email and gmail corporate email being managed by the same user.
