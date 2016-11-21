# Overview

The Hub API allows developers to use the Hub framework to write apps for Broadsoft customers that integrate with all Broadsoft products.  This document outlines how developers can use the API to deliver a Hub application that will enhance the overall experience of the BroadSoft client experience.

## Intended Audience

This document is intended to help developers build and maintain third party Hub integrations. Technical details will be provided that will aid you in calling the Hub API as well as some best practices.

Hub is a part of several products at Broadsoft. (Team-One, CC-One, UC-One) Hub provides a way for developers to build an app that serves up data to the user that enhances an interaction with another user.

To do this, Hub apps have three different areas where data can be delivered: the MicroApp panel, the Notification bar, and the Contextual panel.   

Below is an illustration of how the three components of Hub (Notifications, Contextual, MicroApp) integrate into the UC-One Chrome experience.

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image1.png)

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

The micro app an iframe that you control and host on your servers. The default size of this iframe is 288px by 470px. Users have the ability to expand the window to any height while maintaining a fixed width of 288px.

## User Actions

When users take actions in Hub, your app will be notified and it is up to you to provide data for the user. This may be when a user searches in Hub or clicks on certain buttons in the Hub interface.

## Settings

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image5.png)

When the user first logs into the Hub application, they will see a settings page for all applications. This is where your app will be shown to the users for the first time. The user will attempt to enable your application and then you can start to provide data to that user.
