# Sample Application Guide and Troubleshooting

Check out our hub sample application on github:

https://github.com/BroadsoftLabs/helloWorldSampleApp

## Developing The Hello World Application Locally

The source code for the Hello World app can be downloaded here: https://github.com/BroadsoftLabs/helloWorldSampleApp

The following are instructions for Mac. In some cases you will have to find the instructions for Windows for installing certain packages. All of our development will be done with unix terminal commands.

## Install the Required Dependencies

Firstly, you will need to install git. Please follow this guide: [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

Then install node: [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

The node version compatible with this project is 6.2.2 or later but earlier versions may also work.

## Set up the project

Run this in terminal:

```
git clone [https](https://github.com/BroadsoftLabs/helloWorldSampleApp.git)[://github.com/BroadsoftLabs/helloWorldSampleApp.git](https://github.com/BroadsoftLabs/helloWorldSampleApp.git)

cd helloWorldSampleApp

npm i

bower i

sails lift --verbose
```

Your app should now be running in your browser at [http://localhost:5430/#/](http://localhost:5430/#/)

## Testing Your Application

### Step 1: Register in the hub Developer Portal

NOTE: If you would like to see to see the communicator application for design and demo purposes, it can be Installed here: [https://chrome.google.com/webstore/detail/communicator/ggbjagbpfbiannaconhiblndomfmekkh?hl=en-US&utm_source=chrome-ntp-launcher&authuser=1](https://chrome.google.com/webstore/detail/communicator/ggbjagbpfbiannaconhiblndomfmekkh?hl=en-US&utm_source=chrome-ntp-launcher&authuser=1)

### Step 2: Add a new application with your url on the manage page

Register a sample application in the portal and your settings for your broadsoftlabs account will automatically be updated.

### Step 3: Host your Application

When you register your application, be sure to set the url to your hosted location.

### Step 4: Verify your Code in the Sandbox

Test your code in the micro apps and notifications

### Step 5: Verify Contextual

Test your app in contextual with another user by creating a custom contact

## How Does the Hello World App Work?

The technologies used for this application can be found in the package.json file for the app. [https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/package.json](https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/package.json)

The main technology is SailsJS, a wrapper for express. ([http://sailsjs.org/](http://sailsjs.org/))

The application that you write can be in any technology as long as you can make REST requests to the Hub Core server.

In this application all the rest routes are defined here: [https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/config/routes.js](https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/config/routes.js)

This is where the backend interacts with the Hub API.

Custom authentication to Hub can be viewed here: [https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/api/controllers/OAuthController.js](https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/api/controllers/OAuthController.js)

Hub Core will also query your application with the timeline route for contextual data and the notifications route for the notifications count.

https://github.com/BroadsoftLabs/helloWorldSampleApp/blob/master/api/controllers/HelloWorldController.js
