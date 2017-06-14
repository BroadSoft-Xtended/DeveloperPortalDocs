# API Authorization

When you interact with the Hub api, you will be sending an auth token.

Where auth can be any JSON you want. IE:

```
auth = {
  token: 'g35un4hd7dbdkd876whenjdjkc6dhjsdkysdh',
  date: new Date(),
  organization: 'Verizon',
  verizonUsername: 'TimBingly23'
};
```

It can also be as simple as: `auth = {id: 1234}`

## Step 1 - A user decides to enable your app from settings

Users can select your app from the settings page of Hub. Once they trigger the app to be on, you will be sent the following request that you app must handle:

```
GET

URL: https://myHostedApp.com/authenticate?callback=https://theHubUrlTheUserIsUsing.com
```

When you receive this call, you need to re-direct the user to your signup page.

```
// Save the query params for later
req.session.callback = req.query.callback;

res.writeHead(307, {
  'Cache-Control': 'no-cache, private, no-store, must-revalidate, max-stale=0, post-check=0, pre-check=0',
  Location: '/signup.html'
});
res.end();
```

## Step 2 - Have the user log into your app

Once the user is on signup.html, then you can have them log into your app in any way that you wish. In our example, lets say that they logged in with google and our app got back their google email and an access token that we then encrypted and stored in our database.

## Step 3 - Re-direct the user back to the Hub success page

```
// After signup is complete to google

auth = {
  token: 'hdf8d87fdiujfosdf987sdfujsdflksd98f7',
  googleEmail: 'bobTheBuilder@gmail.com'
}

// In our case, we saved the hubUrl in the session of our app as callback
// username is the name of the user in our 3rd party app

var url = req.session.callback + '?auth=' + JSON.stringify(auth) + '&username=' + username;

return res.redirect(url);
```

## Step 4 - Calls after authenticate

Now, whenever you app is called by Hub, you will get the auth and username as query paramaters. This also includes when Hub renders either the Micro App iframe or the Contextual Iframe.

This also happens when Hub calls your app for notification count.
