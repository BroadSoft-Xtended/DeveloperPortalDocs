# Frequently Asked Questions

## What does contextual really mean?

Contextual data is the data shared between users. This data is shown when a user engages in conversation with another user.

## I can't log into the chrome phone anymore to test my app.

Please try logging out and logging back in. Then try a shutting down all chrome pages and chrome apps after clearing your cache. If you are using
http://xsp.broadsoftlabs.com to log into the chrome phone, you can try using either xsp1 or xsp2 instead of xsp.

If all else fails here, please email us at jodonnell@broadsoft.com to log a ticket.

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
