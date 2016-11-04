# Application Development Process

## Make it

You will need to create a web application that can be displayed in an iframe. Your application should also be capable of sending REST requests if you want to update notifications and contextual information.

## Host it

You can host your application on any public hosting service that you wish. The only thing to consider here is that Hub will have to be able to show this publicly hosted service in an iframe. If you are protecting against CORS, you will also need to add the Hub staging urls and productions urls to your whitelist.

In the case where you are not sure how to host an application on the internet, you can follow the following steps as a guide.

## Hosting on Heroku

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

## App Registration

Once you are pleased with your application and it has been fully tested, you can register your application here: https://hub-sandbox.broadsoftlabs.com/#/app/manage by clicking on build or manage tabs once logged in.

## Test On Staging

You should be testing your application on staging to be sure that notifications and contextual work as intended. During this process, you will be communicating with the Hub team to ensure that your application is working as intended.

## The Hub Team Will Test Your Application

We will be looking for bugs, UI defects, UI general styling and security vulnerabilities. If your application fails any of these steps, you will be contacted by a Hub team member to aid you in fixing these issues before the application can be enabled on production.

## Schedule a Production Release Date

You will work with the Hub team to set a reasonable production release date.
