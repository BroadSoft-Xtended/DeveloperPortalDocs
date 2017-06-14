# Frequently Asked Questions

## What does contextual really mean?

Contextual data is the data shared between users. This data is shown when a user engages in conversation with another user.

## I don't have access to Team-One

You can always get a 30 day free trial with a GMail account here: https://app.intellinote.net/

## Where is the microapp url set

This is the base url you have set in the developer portal for the iFrame.

## I am getting an OPTIONS request. What is that? What is CORS?

CORS is a standard for security on the internet. You can read more about it here: https://github.com/expressjs/cors.

The main part that may confuse you is that POST request that you receive from HUB will all start out as an OPTIONS request. You will need something like this set in your application and you will also need `preflightContinue` set to true.

```
router.options('/*', function(req, res) {
res.send(200, 'CHECKOUT,CONNECT,COPY,DELETE,GET,HEAD,LOCK,M-SEARCH,MERGE,MKACTIVITY,MKCALENDAR,MKCOL,MOVE,NOTIFY,PATCH,POST,PROPFIND,PROPPATCH,PURGE,PUT,REPORT,SEARCH,SUBSCRIBE,TRACE,UNLOCK,UNSUBSCRIBE');
});
```

## Command git not found

Install git on your machine by following this guide: https://www.atlassian.com/git/tutorials/install-git

## Something in heroku is not working

`heroku logs -t` will tail the logs for you.

## Something else?

Email me at jodonnell@broadsoft.com or create an issue here: https://github.com/BroadSoft-Xtended/SampleApps/issues
