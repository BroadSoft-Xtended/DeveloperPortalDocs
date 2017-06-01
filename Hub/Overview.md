# Overview

The Hub API allows developers to use the Hub framework to write apps for Broadsoft customers that integrate with all Broadsoft products.  This document outlines how developers can use the API to deliver a Hub application that will enhance the overall experience of the BroadSoft client experience.

## Intended Audience

This document is intended to help developers build and maintain third party Hub integrations. Technical details will be provided that will aid you in calling the Hub API as well as some best practices.

Hub is a part of several products at Broadsoft. (Team-One, CC-One, UC-One) Hub provides a way for developers to build an app that serves up data to the user that enhances an interaction with another user.

To do this, Hub apps have three different areas where data can be delivered: the MicroApp panel, the Notification bar, and the Contextual panel.   

Below is an illustration of how Hub is displayed in Team-One. Anyone can get a free trial Team-One account. Email us at jodonnell@broadsoft.com if you need your free trial extended.

## Notifications

![](https://raw.githubusercontent.com/BroadSoft-Xtended/DeveloperPortalDocs/master/Hub/images/23.png)

In the notifications bar, you’ll have an icon for your application. This icon is also used for navigation to your app.

### What can I set here?

* App icon for notification uploaded on app registration page (SVG, WOFF)

## Contextual

<img src="https://raw.githubusercontent.com/BroadSoft-Xtended/DeveloperPortalDocs/master/Hub/images/24.png" alt="Drawing" style="width: 600px;"/>

This panel is shown when a user interacts with another user through chat or a phone call. In your application, this is an iframe that is hosted on your app at `/contextual`. This iframe can be resized by the user.

## MicroApp

![](https://raw.githubusercontent.com/BroadSoft-Xtended/DeveloperPortalDocs/master/Hub/images/25.png)

The micro app an iframe that you control and host on your servers. The default size of this iframe is 288px by 470px. Users have the ability to expand the window to any height while maintaining a fixed width of 288px. The url for the MicroApp iframe is the base url of your app that you specify in the developer portal.

## User Actions

When users take actions in Hub, your app will be notified and it is up to you to provide data for the user. This may be when a user searches in Hub or clicks on certain buttons in the Hub interface.

## Settings

![](https://raw.githubusercontent.com/BroadSoft-Xtended/DeveloperPortalDocs/master/Hub/images/image5.png)

When the user first logs into the Hub application, they will see a settings page for all applications. This is where your app will be shown to the users for the first time. The user will attempt to enable your application and then you can start to provide data to that user.

# Application Development Process

## Make it

You will need to create a web application that can be displayed in an iframe. Your application will also need to send REST requests to talk to the API.

## Host it

You can host your application on any hosting service that you wish. The only thing to consider here is that Hub will have to be able to show this publicly hosted service in an iframe. If you are protecting against CORS, you will also need to add the Hub staging urls and productions urls to your whitelist.

In the case where you are not sure how to host an application on the internet, you can follow the following steps as a guide for hosting on Heroku.

## App Registration

While developing your app you can test it by creating an app here: https://developer.broadsoftlabs.com/#/app/make. After you create your application, you can go to the Hub Settings in Team-One to see your application that is private to only you and the list of emails you allow. Once your app is ready for a wider audience, email us at jodonnell@broadsoft.com.

## App Review/Schedule a Production Release

We will be looking for bugs, UI defects, UI general styling and security vulnerabilities. If your application fails any of these steps, you will be contacted by a Hub team member to aid you in fixing these issues before the application can be enabled on production. Click on your app in the make section and then select 'Submit to App directory'. https://developer.broadsoftlabs.com/#/app/appDashboard/hub/submitToDirectory
