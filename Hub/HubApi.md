# UC-One Hub API

Broadsoft Labs

**Version 1.0.3**

Released: July 2016

**The following team members contributed to this document:**

Jonathan O’Donnell

Dominik Steiner

Nathan Stratton

Matt Horning

Amir Mehmood

**About Broadsoft Labs**

Broadsoft Labs is one of the research and development teams within Broadsoft.  The team is focused on bringing new technology ideas to the BroadSoft Product group. UC-One Hub is an idea incubated within Labs to enhance the UC-One client experience and is currently in customer beta.  

You can find out more at: [https://broadsoftlabs.com/](https://broadsoftlabs.com/)

Table Of Contents

[[TOC]]

# Executive Summary

UC-One Hub APIs will allow BroadSoft Service Providers and Partners to leverage the Hub development framework to bring the power of cloud applications to their customers.  This document will outline how developers will engage the API to deliver a Hub application to enhance the overall experience of the BroadSoft client experience.  

# Intended Audience

This document is intended to help developers build and maintain third party Hub integrations. Technical details will be provided that will aid you in calling the Hub API as well as best practices for working with Hub.

# Document Updates

<table>
  <tr>
    <td>Version </td>
    <td>Date</td>
    <td>Summary</td>
  </tr>
  <tr>
    <td>1.0</td>
    <td>Jun 22, 2016</td>
    <td>Updating the titles, formatting and document initialization</td>
  </tr>
  <tr>
    <td>1.0.1</td>
    <td>Jun 27, 2016</td>
    <td>Updating as per a Hub Meeting with more functionality and some specific requirements</td>
  </tr>
  <tr>
    <td>1.0.2</td>
    <td>July 5, 2016</td>
    <td>Updated formatting and content
</td>
  </tr>
  <tr>
    <td>1.03</td>
    <td>July 14, 2016</td>
    <td>Updated the doc sections 6 and 7. Addressed some questions from meetings</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

# 1.0 Overview

UC-One Hub is a BroadSoft cloud service providing direct access to cloud applications that have been pre-integrated with UC-One – allowing users to be more efficient and productive. These integrations bring a lite client experience and contextual data to your day-to-day communications via UC-One.  UC-One Communicator for Desktop-Chrome is the first client to be integrated with Hub, with other client experiences to follow.  

To do this, Hub integrates into the Chrome client experience in three different ways, the MicroApp View, the Notification bar (which also serves as the navigation bar), and the Contextual data view.   

As you build your integration into Hub, your application data will be inserted through the use of a "webview" or iframe elements.

Below is an illustration of how the three components of Hub (Notifications, Contextual, MicroApp) integrate into the UC-One Chrome experience.

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image1.png)

## 1.1 Notifications 

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image2.png)

In the notifications bar, you’ll have an icon for your application, which will act as a reference point for application notifications and navigation. This view is always shown while communicator is open and provides users a heads-up view of items that require their action and a point to navigate to different applications. 

When a user clicks on the notifications icon that corresponds to your app, the MicroApp pane will then shift to your MicroApp pane regardless of where the user is in Communicator at the time.

What can I set here?

* App icon for notification uploaded on app registration page (SVG, WOFF)

* This is elaborated in section 4.2

## 1.2 Contextual

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image3.png)

This panel is shown when an end user interacts with another user through a chat or phone call. When this happens, all enabled apps that have the contextual box checked under settings are sent a request for all user data that they want to display in the contextual view. This data can send a maximum of 30 items on first call with additional content made available utilizing the "more" function. The next request to your API will ask for the next page.

You will be able to display data here in a container 228px by 70px in size. Your icon will also be used here. This is the default size of the window but keep in mind that the user can expand this window to any height while leaving the width constant.

## 1.3 MicroApp

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image4.png)

The micro app is where you can display any content that you would like for your application. You will display this information in the form of an iframe. The url of this iframe can be set when you register your application. It is a scrollable container and the header of this panel will have the name of the application that you registered with Hub. The settings link in the header takes you to the Hub settings page.

The default size of this iframe is 288px by 470px. Users also have the ability to expand the window to any height that their monitor supports while maintaining a fixed width of 288px.

There are a few iframes at work here that you may need to consider. The Communicator application provides Hub with three IFrames so Hub can display its content. As a third party developer, you will only be provided with one IFrame in which to display data (This is the MicroApp IFrame).

All the IFrames in the application talk to each other through two APIs: the internal Hub API and the third party API. Public access is only provided for the third party API.

## 1.5 BroadSearch

BroadSearch is one of the most important parts of Hub as it allows users to easily search through data between all integrations. These can be internal or external applications and allows Hub to combine data of all types in a global search.

There are two places that the user can use the BroadSearch feature: In the micro apps view and in the contextual view. However, when the user searches in the micro apps view, only the current application is searched. In contextual however, search behaves on a more global level.

In contextual, if you search the while in the "All" tab, you will search all applications at once. As you filter the contextual view to categories or subcategories, you then start to limit the scope of the BroadSearch feature to the desired applications.

## 1.6 Settings

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image5.png)

When the user first logs into the Hub application, they will see a settings page for all applications.

Here you can see the settings page. This is an example of a user only having the first two integrations enabled. 

Once an integration is enabled, the blue slider toggles to "ON" and more details become available. You can then click the “Launch” button to navigate to the integration micro app view.

You can also toggle where you want the application data displayed. By disabling contextual, your data will no longer be available in the contextual view. By disabling notifications, you will disable the notification counts and updates. Disabling this does not remove the application from the notifications panel entirely as it will still be used when a user clicks the application 

icon corresponding the affected application and Hub navigates 

them to the micro app view for that application.

The user can also view which account is linked to this integration. This is helpful if you want to determine where the data is coming from in the case where a user might have multiple accounts for a single application. A good example of this is gmail personal email and gmail corporate email being managed by the same user.

# 2.0 Best Practices

Your application code will need to be submitted for review by a member of the Broadsoft Labs team. This review process may take some time so it is in everyone’s best interest to get your app in the best state possible prior to submitting the app for approval. Here are some things that will be reviewed in this process and some tips on how to minimize the review process time.

## 2.1 Approved Frameworks

This is the list of approved frameworks. Your application will not be approved if it does not follow a modern framework. If you are using a framework that is not in this list, please contact us to find out if that framework is approved.

<table>
  <tr>
    <td>Node.JS
Hapi.js
Sails.js
Express.js
Koa.js
Mojito.js



Java
Spring
JSF
Vaadin


Scala
Lift
Play
</td>
    <td>
Meteror.js
Derby.js
Mean.js
Total.js







GWT
Grails
Play
</td>
    <td>PHP
Drupal
Cake.php
Laravel
Symfony
CodeIgniter
Yii 2


Python
Django
TurboGears
Web2py



Clojure
Luminus

</td>
    <td>
Phalcon
Zend
Slim
Fuel
PhPixie
</td>
    <td>Ruby
Ruby on Rails
Sinatra






GO
Beego
Martini
Gorilla
GoCraft


Microsoft
All Frameworks Accepted
</td>
  </tr>
</table>


** More frameworks can be supported upon request. Please contact us if your framework does not fall into this list. Front-end frameworks such as Angular, React, Backbone etc. are also approved.

## 2.2 Performance

**Minification/Uglification/Concatenation**

While your app will have to be submitted in non-uglified mode, all apps running in our production environment should be minified and uglified. This increases performance and reduces initial load times. We should only ever be serving one file to the user for the client side portion of your application.

## 2.3 Security

**HTTPS**

Your app must be hosted on Https to interface with Hub.

**XSS**

Your code should be protected against XSS (Cross site scripting attacks). 

XSS is a type of [computer security](https://en.wikipedia.org/wiki/Computer_security) [vulnerability](https://en.wikipedia.org/wiki/Vulnerability_(computer_science)) typically found in [web applications](https://en.wikipedia.org/wiki/Web_application). XSS enables attackers to [inject](https://en.wikipedia.org/wiki/Code_injection) [client-side scripts](https://en.wikipedia.org/wiki/Client-side_script) into [web pages](https://en.wikipedia.org/wiki/Web_page) viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass [access controls](https://en.wikipedia.org/wiki/Access_control) such as the [same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy). Cross-site scripting carried out on websites accounted for roughly 84% of all security vulnerabilities.

Their effect may range from a petty nuisance to a significant security risk, depending on the sensitivity of the data handled by the vulnerable site and the nature of any security mitigation implemented by the site's owner.

(*) https://en.wikipedia.org/wiki/Cross-site_scripting

**CSRF**

Another common JavaScript security vulnerability is [Cross-Site Request Forgery](http://www.veracode.com/security/csrf) (CSRF). This is something your app should prevent.

Cross-site request forgery, also known as one-click attack or session riding and abbreviated as CSRF, is a type of malicious [exploit](https://en.wikipedia.org/wiki/Exploit_(computer_science)) of a [website](https://en.wikipedia.org/wiki/Website) where unauthorized commands are transmitted from a [user](https://en.wikipedia.org/wiki/User_(computing)) that the website trusts. Unlike [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) (XSS), which exploits the trust a user has for a particular site, CSRF exploits the trust that a site has in a user's browser.

(*) https://en.wikipedia.org/wiki/Cross-site_request_forgery

**CORS**

Where applicable, your app should have restricted CORS (Cross origin resource sharing) domains. This will prevent unwanted requests from unwanted domains. Since our Hub servers will send requests to your server, you will need to whitelist the sandbox, staging and production urls for Hub.

Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a [web page](https://en.wikipedia.org/wiki/Web_page) to be requested from another [domain](https://en.wikipedia.org/wiki/Domain_name) outside the domain from which the resource originated.

A web page may freely embed images, [stylesheets](https://en.wikipedia.org/wiki/Style_sheet_(web_development)), scripts, [iframes](https://en.wikipedia.org/wiki/Iframes), videos and some plugin content (such as [Adobe Flash](https://en.wikipedia.org/wiki/Adobe_Flash)) from any other domain. However, embedded [web fonts](https://en.wikipedia.org/wiki/Web_fonts) and [AJAX](https://en.wikipedia.org/wiki/AJAX)([XMLHttpRequest](https://en.wikipedia.org/wiki/XMLHttpRequest)) requests have traditionally been limited to accessing the same domain as the parent web page (as per the [same-origin security policy](https://en.wikipedia.org/wiki/Same-origin_policy)). "Cross-domain" AJAX requests are forbidden by default because of their ability to perform advanced requests (POST, PUT, DELETE and other types of HTTP requests, along with specifying custom [HTTP headers](https://en.wikipedia.org/wiki/HTTP_headers)) that introduce many cross-site scripting security issues.

CORS defines a way in which a browser and server can interact to determine safely whether or not to allow the cross-origin request. It allows for more freedom and functionality than purely same-origin requests, but is more secure than simply allowing all cross-origin requests. It is a recommended standard of the [W3C](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium).

(*) https://en.wikipedia.org/wiki/Cross-origin_resource_sharing

## 2.4 App languages/localization

Hub will support various languages worldwide. Your app must support a base set of these languages for all strings presented to the users. Your app will also be required to listen for HTTP calls to inform your app of when the user has changed their language in Hub. Then you will be required to update your user interface to reflect the selected language.

Currently supported languages:

* English

Future Supported Languages: (Estimated date is end of Q3 2016)

* German

* Spanish

* French

* Italian

* Mandarin

## 2.5 Style guide

For a Hub app to be approved it must be submitted to the Hub UI team for approval.  While we provide developers the creative liberty to build a UI that they believe will be welcomed by their user based, we ask that you consider the Google material design as a guide as you follow the style guidelines as prescribed below.   If the UI doesn’t followed modern web standards, like Google’s Material design, then there’s a strong likelihood that the design will be pushed back for remediation.



2.5.1 Typography

Text should be understandable by anyone, anywhere, regardless of their culture or language. Clear, accurate, and concise text makes interfaces more usable.

Open Sans is the standard typeface for UC-One Hub, the font stack should specify Open Sans, Arial, and then sans-serif.

**MicroApp Search Box**

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image6.png)

**MicroApp Tabs Filter**

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image7.png)

**MicroApp List Item**

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image8.png)

**Contextual Timeline Item **

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image9.png)

2.5.2 Colors

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image10.png)

## 2.6 Gathering data from users

You may track user statistics from services such as google analytics in any way you see fit for user interactions that pertain to your application within the micro apps iframe. 

You may not store any information that relates to user interaction with Hub or Communicator. This includes how the user interacts with notifications, contextual or as they switch between apps in Hub. You must also never track the user by any means that will allow you to profile a user and store user information. All data must be anonymized through one way encryption.

Please see the full list of requirements in the terms and conditions section of this document.

## 2.7 Logging

We recommend that you follow standard logging practices for your application. This should involve logging to a file on your servers. Any client side logging must be limited for the production level app as it will be shown to the user if the inspect their chrome console. Take care not to log any sensitive data.

However, you need to make sure that you are not logging excessively as you can crash Hub with an infinite loop of log statements. If your app causes performance issues or impacts performance negatively in any way, your application will be blacklisted from Hub. We will do this by disabling your application globally our production database.

## 2.8 Refreshing Data

### 2.8.1 Contextual and Notifications

When Hub sends your application a request for data in contextual and notifications, there is an option you return called "refreshAt" (See section 4). This property will tel Hub when to next fetch data from your application in order to refresh the contextual panel for the user and the notifications count.

### 2.8.2 MicroApp

You have complete control of when the iframe for your micro app refreshes. The best practice is to update the data when new user data becomes available. It is standard practice to do this with websockets. The screen should never flicker with new data and you should never see the page reload. Rather the data should just update as it becomes available to the user.

## 2.9 More Buttons

If you are providing more than thirty results to a user, your data must be paged. This means that you will only send the first thirty results on first request and as the user request more data, you will serve up the next set of results. This can take place in the form of a more button that the user clicks or from a scrolling action of the user when they scroll to the bottom of the available results. 

This is in place as there are several apps loading data and it is better to only serve a small set of results to limit user bandwidth and increase performance while loading the application initially.

## 2.10 Searching Data

When a user attempts to search your data that you provide in your microapp, you should attempt to provide search results as quickly as possible. Your application should search all front end results initially and filter them on the front end for the user to view. This should be the first  stage of your search. While you are filtering your front end results, you should also search your backend for the data as well and once the results come back from your server, you will need to merge the two searches.

## 2.11 Sub-Filters for MicroApp Data

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image11.png)

If you wish to filter your data into multiple views you will need to have a sun-filters in your micro app.

The current tab should be highlighted and clicking on other tabs should filter the view. Data for each tab should be cached so that when the user switches tabs, the screen does not flash and a reload is not required.

## 2.12 Other User Interface Elements not Discussed

Your application may have several other types of widgets and tools in order to improve the user experience. This section will be updated as needed with a standard on those types of tools and we are fully willing to work with you to make your application the best it can be. The goal of this section is to have a standard way of doing things for all Hub apps in order to present the user with consistent results.

# 3.0 Register Your App

Before you can update any data within Hub, you must register your application. To do this, send an email to hub@broadsoftlabs.com (in the future, this will be done through a web portal) with the following details:

**To**: [hub@broadsoftlabs.com](mailto:hub@broadsoftlabs.com)

**Subject**: Hub registration for app: <your-app-name>

**Body**:

* **Company: **Name of Company or service provider that is providing the app.

* **App N****ame**: The name of your application.

* **Type of App: **Microapp, Contextual, MicroApp Only, or Contextual Only.

* **App Category:** What category does the app belong? You must choose from: Dialogue, Files, Social, Teams, Utilities.

* **Sub-category:** Provide the name of your subcategory. This is limited to 10 characters.

* **Icon**: This is the icon that we will display in the notifications panel of Hub. It must be in the form of a WOFF file. The icon in your font file must be set to the \e0 character

* **Icon Color**: Hex code of your preferred color. Ex: #f00

* **Scope: **Is this app offered to service provider, or everyone?

* **Is This a Public API**: This is a true or false value that indicates if you would like to send private user data to Hub. This requires an authentication method that Hub can access when making requests.

* **Base URL**: Where your application or API is publicly hosted. This may also include a port if applicable. This is referenced as <integration.host>:<port> in this document.

# 4.0 API overview

Once your application is registered, you'll be able to interface with Hub. As the user takes actions in Hub, we will send you requests. You then respond to those requests with data that we show in Contextual and Notifications. We also provide a mechanism for you to send us updates to your data as they become available to the user.

## 4.1 Authentication

As a developer, you are to send and receive secure requests. The way this works is that Hub will request a secure key on a per user basis. This key can be any string that you wish to send to Hub. Hub will then send this string back to your application for this user each time secure interactions are required. However, if it is actual user data, such as an access_token or a secure key, we recommend that you encrypt this key before sending it to us.

Once you inform Hub of the key and associated user, we will then send requests back to you with that same key. This is how you will determine if it is a secure request.

### 4.1.1 OAuth 2

Hub supports OAuth 2.0, which allows you to log the user into your application with Google, Facebook, LinkedIn, etc. and you may then want to send us the unique id that matches one of these services. For instance, if your login flow produces some sort of user token, you could then encrypt that token and send it to Hub. Hub will then send you that token back verbatim on each request and it will be your responsibility to decrypt and verify that key. This will ensure that the request to your server is actually coming from Hub and not a malicious source.

### 4.1.2 Custom Tokens

Another example is where you may just want a custom auth token that you use to validate with Hub. This custom token can literally be any string you wish. Some sample ideas for this token could be the username, user’s email, a timestamp of the first interaction with Hub or even just a static string encrypted with a secret only your application knows. This is a less secure mechanism but it may fit your needs better depending on your application.  

You can encode these tokens in any way you wish. The following examples are encrypted with base64 encoding:

<table>
  <tr>
    <td>Properties</td>
    <td>Un-encoded Key</td>
    <td>Encoded Key</td>
  </tr>
  <tr>
    <td>email</td>
    <td>user@sample.com</td>
    <td>dXNlckBzYW1wbGUuY29t</td>
  </tr>
  <tr>
    <td>Email + Current Time</td>
    <td>user@smaple.com20160808</td>
    <td>dXNlckBzbWFwbGUuY29tMjAxNjA4MDgNCg==</td>
  </tr>
  <tr>
    <td>User GUID from Database</td>
    <td>HDD7YDHEJ7353HDHHS</td>
    <td>SEREN1lESEVKNzM1M0hESEhT</td>
  </tr>
</table>


### 4.1.3 How we will attempt to retrieve a custom token

The following will occur if your application is set to Private upon registration and when a user tries to log in to your app from the settings page. We will open a page pointing to your server at:

```
GET

URL: https://<yourBaseUrl>/v1/{yourAppName}/authenticate?hubUrl=[https://core.broadsoftlabs.com](https://core.broadsoftlabs.com)

and you can render your login page or any OAuth integration with other providers in at that time.

Upon successful login, you will send us the encrypted authentication token as a POST

POST

URL: https://<hubUrl>/v1/{yourAppName}/:username/auth

Post Body: {

  auth: <any>

}
```

Hub will store this token and associate with your application and a user. Then this token will be sent with all Hub requests. 

We will respond to the POST with a URL that you should be redirecting to in order to display the UC-One Hub authentication success or error page.

## 4.2 How to Update the Notifications Count:

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

## 4.3 How to Update Contextual Data

When the user opens a conversation with one or many users, we will send you the following post request:

![](https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/Hub/images/image12.png)

```
POST

URL: https://<yourBaseUrl>/v1/:app/:username/timeline <private API>
```

OR

```
URL: https://<yourBaseUrl>/v1/:app/timeline <public API>

Post Body: 

{

  auth: <any> (optional),

  search: <string>, (optional) (this is the string that the user searched)

  context: { (details of the person or persons that you are taking to)

    emails: <array>,

    phoneNumbers: <array>

  }

}

This request will hit your API and you will respond with:

{ 

  refreshAt: <integer in ms>, (optional)

  items: 

[

     {

date: <Time since Jan 1 1970 UTC>,

title: <string> (first row),

description: <string> (optional)

timelineId: <string> (unique id that represents the record),

webLink: <string> (open in browser url),

actions: [

{

property: <string> (name of the boolean property of the timeline item),

iconClass: <string> (name of the icon to be displayed through HubIconFont’s provided css classes),

Color: <string> (css color of the activated action)

}

] (optional)

     }, 

     {

/* Similar object */

     }

 ]

}
```

The **Auth** we send you will have been attained by Hub when a user activates your private application from the Hub settings page.

**Search** is the query string that the user has searched for in contextual.

**Context** contains the details of the other users that the target user is talking to.

	**Emails** is a list of all user emails in the current user conversation.

	**Phone Numbers** is a list of all user phone numbers in the conversation.

**RefreshAt** is the next time (UTC) that we will poll your application for changes. Once that request is sent, you will need to return the next time you want us to check your app for changes and so on.

Timeline is the list of data that you want displayed in contextual. This is an array of objects.

* Date is the date you want displayed. The accepted format is the time since Jan 1 1970 UTC

* Title is the first bolded row of the contextual record

* Sub title is the optional second row of the contextual record

* Description is the one to three lines of details that you may provide for each timeline item.

* Timeline Id is a unique id in your system that represents the timeline item.

* WebLink is the url the link that is made available so a user can click your timeline item.

* Actions are icons that you want shown as the user hovers over your contextual item. By specifying them in the actions property of the timeline response you can toggle a boolean property of your timeline items. On clicking the icon we will send a PUT request to 

https://<baseUrl>/v1/:app/:username/timeline/:timelineId/:property

With the toggled value of the property in the body of the request. 

## 4.4 Push notifications update of Hub Contextual and Notifications

Whenever this route is called, Hub will attempt to call your application for notifications and contextual data and update the user interface with that new data.

Please send us the following POST request:

POST

URL: [https://](https://core.broadsoftlabs.com/v1/:app/:username/push)[core.broadsoftlabs.com](https://core.broadsoftlabs.com/v1/:app/:username/push)[/v1/:app/:username/push](https://core.broadsoftlabs.com/v1/:app/:username/push)

# 5.0 Example Application Development Process

## 5.1 Make it

You will need to create a web application that can be displayed in an iframe. Your application should also be capable of sending REST requests if you want to update notifications and contextual information.

## 5.2 Host it

You can host your application on any public hosting service that you wish. The only thing to consider here is that Hub will have to be able to show this publicly hosted service in an iframe. If you are protecting against CORS, you will also need to add the Hub staging urls and productions urls to your whitelist. 

In the case where you are not sure how to host an application on the internet, you can follow the following steps as a guide.

### Heroku

[https://devcenter.heroku.com/start](https://devcenter.heroku.com/start)

Download the heroku toolbelt here: [https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up)

In terminal write: 

```
Mkdir hostingTester

Cd hostingTester

git init

vi index.html

Npm init

git add .

git commit -am 'my first commit'

heroku create

git push heroku master
```

Then follow the links in the heroku output to see your application hosted online. For more details please visit heroku.com.

## 5.3 App Registration

Once you are pleased with your application and it has been fully tested, you can register your application with Broadsoft Labs as previously described. The Hub team will review your application and then enable your application on our Hub staging servers where you can fully test the integrated solution.

## 5.4 Test On Staging

You should be testing your application on staging to be sure that notifications and contextual work as intended. During this process, you will be communicating with the Hub team to ensure that your application is working as intended.

## 5.5 The Hub Team Will Test Your Application

We will be looking for bugs, UI defects, UI general styling and security vulnerabilities. If your application fails any of these steps, you will be contacted by a Hub team member to aid you in fixing these issues before the application can be enabled on production.

## 5.6 Schedule a Production Release Date

You will work with the Hub team to set a reasonable production release date.

# 6.0 Hub Developer Portal (Coming Soon)

In this tool, you will be able to fully test your application while only needing your own local code to do so. Here you will register applications and schedule releases for your product. This is also where you will receive feedback during the review process.

# 7.0 Sample Application Guide and Troubleshooting

## 7.1 Developing The Hello World Application Locally

The source code for the Hello World app can be downloaded here: https://github.com/BroadsoftLabs/helloWorldSampleApp

The following are instructions for Mac. In some cases you will have to find the instructions for Windows for installing certain packages. All of our development will be done with unix terminal commands.

### Step 1: Install the Required Dependencies

Firstly, you will need to install git. Please follow this guide: [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

Then install node: [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

The node version compatible with this project is 6.2.2 or later but earlier versions may also work.

### Step 2: Set up the project

Run this in terminal:

```
git clone [https](https://github.com/BroadsoftLabs/helloWorldSampleApp.git)[://github.com/BroadsoftLabs/helloWorldSampleApp.git](https://github.com/BroadsoftLabs/helloWorldSampleApp.git)

cd helloWorldSampleApp

npm i

bower i

sails lift --verbose
```

Your app should now be running in your browser at [http://localhost:5430/#/](http://localhost:5430/#/)

## 7.2 Testing Your Application (Coming Soon)

### Step 1: Register in the hub Developer Portal

NOTE: If you would like to see to see the communicator application for design and demo purposes, it can be Installed here: [https://chrome.google.com/webstore/detail/communicator/ggbjagbpfbiannaconhiblndomfmekkh?hl=en-US&utm_source=chrome-ntp-launcher&authuser=1](https://chrome.google.com/webstore/detail/communicator/ggbjagbpfbiannaconhiblndomfmekkh?hl=en-US&utm_source=chrome-ntp-launcher&authuser=1)

### Step 2: Create BroadsoftLabs Account

Sign up for an account at broadsoftlabs.com. NOTE: You will need a corporate email address from Broadsoft.com or a Broadsoft partner in order to go any further.

### Step 3: Register a Sample Application 

Register a sample application in the portal and your DMS config settings for your broadsoftlabs account will automatically be updated.

### Step 4: Host your Application

When you register your application, be sure to set the url to your hosted location.

### Step 5: Verify your Code in the Sandbox

Test your code in the micro apps and notifications

### Step 6: Verify Contextual

Test your app in contextual with another user by creating a custom contact

## 7.3 How Does the Hello World App Work?

The technologies used for this application can be found in the package.json file for the app. [https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/package.json](https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/package.json)

The main technology is SailsJS, a wrapper for express. ([http://sailsjs.org/](http://sailsjs.org/))

The application that you write can be in any technology as long as you can make REST requests to the Hub Core server.

In this application all the rest routes are defined here: [https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/config/routes.js](https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/config/routes.js)

This is where the backend interacts with the Hub API.

Custom authentication to Hub can be viewed here: [https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/api/controllers/OAuthController.js](https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/api/controllers/OAuthController.js)

Hub Core will also query your application with the timeline route for contextual data and the notifications route for the notifications count. 

https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/api/controllers/HelloWorldController.js 

# 8.0 Working with your BroadsoftLabs Account

Log in to broadsoftlabs.com

Then navigate here: https://broadsoftlabs.com/thelab/accounts

You can either use an account that you have listed, or you can create a new one. ![](https://puu.sh/qf206/6bc10d1688.png)

![](https://puu.sh/qf20K/7f96fc18b6.png)

Then you need to manually set the password on this page: https://broadsoftlabs.com/thelab/accounts

Click the crayon edit button on hover and you can then update your password

![](https://puu.sh/qf23u/a53fe0c2ed.png)

Then click the green check mark. Now you should be able to log into the dev portal with these two fields: ![](https://puu.sh/qf26m/5f069f4697.png)
