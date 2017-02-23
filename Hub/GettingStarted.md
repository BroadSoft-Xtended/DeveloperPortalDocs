# Getting started

This will get you going with the most basic Hub app. These instructions are for MacOS. You will need brew installed if you don't have all the required dependencies. `https://brew.sh/`. In this example, we will be building an app in [Node.js](https://nodejs.org/en/) with [Express.js](http://expressjs.com/). We will be hosting it on [Heroku](https://www.heroku.com/) for testing. You will also need the heroku tool belt which you can get here: https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up

For more info, see the overview section of these docs.

## Important Notes

- Your app has to be publicly hosted for Hub to be able to work with it.
- Your app must be hosted with https

## Steps

1. Clone our sample apps repository.

`git clone https://github.com/BroadSoft-Xtended/sample-apps.git`

2. Navigate to the project you want to test out.

`cd sample-apps/hub/simple`

3. Push this code to heroku to host it for testing

```
rm -rf .git
npm install
git init && git add . && git commit -am 'my first commit'
heroku create && git push heroku master
```

Then follow the links in the heroku output to see your application. You should see something like this: ![](https://puu.sh/ueT0s/cfbf22dc42.png) where my url was https://infinite-beach-91227.herokuapp.com/

4. Create a Hub App in the Dev Portal

Go here and log in. https://developer.broadsoftlabs.com/#/app/login

If you don't have a Team-One account yet, don't worry, you can get one free for 30 days. If you need longer than 30 days email me at jodonnell@broadsoft.com

Once you have logged in to Team-One, go to: https://developer.broadsoftlabs.com/#/app/make and click the 'Create New App' button in the Hub section. Enter an application name that means something to you and the URL you got from heroku. Make sure the url is in https form.

You should now see an app like this: ![](https://puu.sh/ueTmj/a7bcd5c8ce.png). Click the app to see the app settings.

5. See your app in Team-One

Go to https://app.intellinote.net/

Click on the 'Manage Integrations' button in the bottom left ![](https://puu.sh/ueTsE/1bef11f894.png)

Click on 'Manage Hub Settings' ![](https://puu.sh/ueTuI/250e4dcb16.png)

Once the list loads, scroll down to see your application and toggle it on. ![](https://puu.sh/ueTEp/631675a201.png)

Go back to https://app.intellinote.net and go to your personal workspace. ![](https://puu.sh/ueTPE/9c843f435a.png)

Click your app: ![](https://puu.sh/ueTRd/1939daffdd.png) and see it in action! ![](https://puu.sh/ueTSl/55f8430417.png)

You can view contextual information by adding a user to your organization or talking to an existing user.

6. Get contextual working

Click on the contextual tab at the top of the user conversation ![](https://puu.sh/ueTXm/590183895a.png) and see your app. ![](https://puu.sh/ueTZi/f962e859f1.png)

You will notice that there are no contextual items that show up. That is because the code calls the app 'HelloWorld' and you named it something different. For me, I would have to replace all instances of 'helloWorld' in app.js with 'testjonnn'

Run this command with your app name.

```
sed -i '' 's#helloWorld#testjonn#' app.js
git add . && git commit -am 'getting contextual working'
git push heroku master
```

## Its not working for me

### Command git not found

Install git on your machine by doing `brew install git`

### Something in heroku is not working

` heroku logs -t` will tail the logs for you.

### Something else?

Email me at jodonnell@broadsoft.com
