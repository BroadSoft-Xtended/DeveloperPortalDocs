# Best Practices

Your application code will need to be submitted for review by a member of the Broadsoft Labs team. This review process may take some time so it is in everyone’s best interest to get your app in the best state possible prior to submitting the app for approval. Here are some things that will be reviewed in this process and some tips on how to minimize the review process time.

## Approved Frameworks

Your application will not be approved if it does not follow a modern web framework. If you are not sure if your framework meets these requirements, please contact us.

## Logging

We recommend that you follow standard logging practices for your application. This should involve logging to a file on your servers. Any client side logging must be limited for the production level app as it will be shown to the user if they inspect their chrome console. Take care not to log any sensitive data.

## Performance

**Minification/Uglification/Concatenation**

While your app will have to be submitted in non-uglified mode, all apps running in our production environment should be minified and uglified. This increases performance and reduces initial load times. We should only ever be serving one file to the user for the client side portion of your application.

## Security

### HTTPS

Your app must be hosted on Https to interface with Hub.

### Other considerations

Your code should prevent the following attacks: [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting), [Cross-Site Request Forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery), [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Most modern frameworks have simple ways to greatly increase the security of your application.
