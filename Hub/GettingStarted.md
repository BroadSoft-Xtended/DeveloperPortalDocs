# Getting started

This will get you going with the most basic Hub app. These instructions are for MacOS but should work fine with other operating systems. In this example, we will be building an app in [Node.js](https://nodejs.org/en/) with [Express.js](http://expressjs.com/). We will be hosting it on [Heroku](https://www.heroku.com/) for testing. You will also need the heroku tool belt which you can get here: https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up.

For more info, see the overview section of these docs.

Download Node.js [here](https://nodejs.org/en/download/)

## Important Notes

- Your app has to be publicly hosted for Hub to be able to work with it.
- Your app must be hosted with https
- The app name that you use in your code and in the hub Routes must match the name of the app that you create in the developer portal.
(Note: all routes in the sample project have .../:appName/... in the route declaration. This is so your app can be flexible on the name.)

## Steps

### 1. Clone our sample apps repository.

`git clone https://github.com/BroadSoft-Xtended/SampleApps.git`

### 2. Navigate to the project you want to test out.

`IE: cd SampleApps/Hub/HelloWorld`

### 3. Push this code to heroku to host it for testing

Now you will need to set up a new project with Heroku. We are doing this so we can host our app on a public domain easily so that Hub can read it from the cloud.

```
rm -rf .git //Make sure that you do not have a current git directory
npm install //In the file: package.json, we have all the app dependencies set up to download
git init && git add . && git commit -am 'my first commit' //This creates a new git repository locally
heroku create && git push heroku master //This creates a new Heroku instance in the cloud and pushes your code to that instance.
```

Then follow the links in the heroku output to see your application. You should see something like this where my url was https://infinite-beach-91227.herokuapp.com/

<img src="https://puu.sh/ueT0s/cfbf22dc42.png" alt="Drawing" style="width: 600px;"/>

### 4. Create a Hub App in the Dev Portal

Go here and log in. https://developer.broadsoftlabs.com/#/app/login

If you don't have a Team-One account yet, don't worry, you can get one free for 30 days. If you need longer than 30 days email me at jodonnell@broadsoft.com

Once you have logged in to Team-One, go to: https://developer.broadsoftlabs.com/#/app/make and click the 'Create New App' button in the Hub section. Enter an application name that means something to you and the URL you got from heroku. Make sure the url is in https form.

You should now see an app like this:

<img src="https://puu.sh/ueTmj/a7bcd5c8ce.png" alt="Drawing" style="width: 600px;"/>

Click the app to see the app settings.

### 5. See your app in Team-One

Go to https://app.intellinote.net/

Click on the 'Manage Integrations' button in the bottom left

<img src="https://puu.sh/ueTsE/1bef11f894.png" alt="Drawing" style="width: 200px;"/>

Click on 'Manage Hub Settings'

<img src="https://puu.sh/ueTuI/250e4dcb16.png" alt="Drawing" style="width: 200px;"/>

Once the list loads, scroll down to see your application and toggle it on.

<img src="https://puu.sh/ueTEp/631675a201.png" alt="Drawing" style="width: 600px;"/>

Go back to https://app.intellinote.net and go to your personal workspace.

<img src="https://puu.sh/ueTPE/9c843f435a.png" alt="Drawing" style="width: 200px;"/>

Click your app:

<img src="https://puu.sh/ueTRd/1939daffdd.png" alt="Drawing" style="width: 200px;"/>

and see it in action!

<img src="https://puu.sh/ueTSl/55f8430417.png" alt="Drawing" style="width: 200px;"/>

You can view contextual information by adding a user to your organization or talking to an existing user.

### 6. Get contextual working

Click on the contextual tab at the top of the user conversation

<img src="https://puu.sh/ueTXm/590183895a.png" alt="Drawing" style="width: 600px;"/>

and see your app.

<img src="https://puu.sh/ueTZi/f962e859f1.png" alt="Drawing" style="width: 600px;"/>

You may notice that there are no contextual items that show up. If this happens, please check the route definitions in app.js to make sure they match the app name you created or that they take the appName anonymously. (IE: ../:appName/...)

Run this command with your app name.

## 7. Push an update to your

As you continue to improve your app, you will need to update your codebase and push it to heroku. This can be done with the following two commands:

```
git add . && git commit -am 'getting contextual working'
git push heroku master
```

## Its not working for me

### Command git not found

Install git on your machine by following this guide: https://www.atlassian.com/git/tutorials/install-git

### Something in heroku is not working

` heroku logs -t` will tail the logs for you.

### Something else?

Email me at jodonnell@broadsoft.com or create an issue here: https://github.com/BroadSoft-Xtended/SampleApps/issues
