# Application Development Process

## Make it

You will need to create a web application that can be displayed in an iframe. Your application will also need to send REST requests to talk to the API.

## Host it

You can host your application on any hosting service that you wish. The only thing to consider here is that Hub will have to be able to show this publicly hosted service in an iframe. If you are protecting against CORS, you will also need to add the Hub staging urls and productions urls to your whitelist.

In the case where you are not sure how to host an application on the internet, you can follow the following steps as a guide for hosting on Heroku.

## Hosting on Heroku

[https://devcenter.heroku.com/start](https://devcenter.heroku.com/start)

Download the heroku toolbelt here: [https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up)

In terminal write:

```
Mkdir hostingTester
Cd hostingTester
git ini
vi index.html
npm init
git add .
git commit -am 'my first commit'
heroku create
git push heroku master
```

Then follow the links in the heroku output to see your application. For more details please visit heroku.com.

## App Registration

Once you are pleased with your application and it has been fully tested, you can register your application here: https://developer.broadsoftlabs.com/#/app/make. After you register your application, you can go to user settings to see your login credentials: https://developer.broadsoftlabs.com/#/app/settings. Your app is now hosted on Hub staging.

## Schedule a Production Release

We will be looking for bugs, UI defects, UI general styling and security vulnerabilities. If your application fails any of these steps, you will be contacted by a Hub team member to aid you in fixing these issues before the application can be enabled on production. Click on your app in the make section and then select 'Submit to App directory'. https://developer.broadsoftlabs.com/#/app/appDashboard/hub/submitToDirectory
