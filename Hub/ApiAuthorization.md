# API Authorization

Once your application is registered, you'll be able to interface with Hub. As the user takes actions in Hub, we will send you requests. You then respond to those requests with data that we show in Contextual and Notifications. We also provide a mechanism for you to send us updates to your data as they become available to the user.

## Authentication

As a developer, you are to send and receive secure requests. The way this works is that Hub will request a secure key on a per user basis. This key can be any string that you wish to send to Hub. Hub will then send this string back to your application for this user each time secure interactions are required. However, if it is actual user data, such as an access_token or a secure key, we recommend that you encrypt this key before sending it to us.

Once you inform Hub of the key and associated user, we will then send requests back to you with that same key. This is how you will determine if it is a secure request.

## OAuth 2

Hub supports OAuth 2.0, which allows you to log the user into your application with Google, Facebook, LinkedIn, etc. and you may then want to send us the unique id that matches one of these services. For instance, if your login flow produces some sort of user token, you could then encrypt that token and send it to Hub. Hub will then send you that token back verbatim on each request and it will be your responsibility to decrypt and verify that key. This will ensure that the request to your server is actually coming from Hub and not a malicious source.

## Username

In many hub routes, you will need to have a username sent. You may also receive this username. This is the username that Hub has for your user. An example could be that you had sent the username to Hub for authentication. Once this happens, this is the username that Hub will send back to you with each call where applicable.

## Custom Tokens

Another example is where you may just want a custom auth token that you use to validate with Hub. This custom token can literally be any string you wish. Some sample ideas for this token could be the username, user’s email, a timestamp of the first interaction with Hub or even just a static string encrypted with a secret only your application knows. This is a less secure mechanism but it may fit your needs better depending on your application.  

## How we will attempt to retrieve a custom token

The following will occur if your application is set to Private upon registration and when a user tries to log in to your app from the settings page. We will open a page pointing to your server at:

```
GET

URL: https://<yourBaseUrl>/<yourAppName>/authenticate?hubUrl=<theUrlWhereHubIsHosted>&hubLoginToken=<theHubLoginTokenForUser>
```

You will need to associate this token with a user in your database. This is because hub will send that token back to you on each request. You will need to match the hubLoginToken to a user to know which user hub is requesting data for.

Then you can render your login page or any OAuth integration with other providers in at that time.
Upon successful login, you will send us the encrypted authentication token as a POST

```
POST

URL: https://<hubUrl>/<yourAppName>/:username/auth

Post Body: {
  auth: 'my custom auth string'
}
```

After this POST returns a success message, you will need to redirect the user to the Hub success page.

```
  var options = {
    method: 'POST',
    uri: 'https://core.broadsoftlabs.com/v1/' + req.session.appName + '/jodonnell@broadsoft.com/auth',
    body: {
      hubLoginToken: req.session.hubLoginToken,
      auth: 'jodonnell@broadsoft.com'
    },
    json: true
  };

  // Once you successfully send that request off to hub, you have to redirect the user to the url that hub tells you
  // This will show the user the Hub login success page.
  rp(options).then(function(result) {
    res.redirect(result.url);
  }).catch(function(error) {
    console.log('Could not post to hub', error.message);
    res.send(500, error);
  })
```

Hub will store this token and associate with your application and a user. Then this token will be sent with all Hub requests.

We will respond to the POST with a URL that you should be redirecting to in order to display the Hub authentication success or error page.
