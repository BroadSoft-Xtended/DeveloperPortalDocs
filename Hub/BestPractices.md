# UC-One Hub API - Best Practices

Your application code will need to be submitted for review by a member of the Broadsoft Labs team. This review process may take some time so it is in everyone’s best interest to get your app in the best state possible prior to submitting the app for approval. Here are some things that will be reviewed in this process and some tips on how to minimize the review process time.

## Approved Frameworks

This is the list of approved frameworks. Your application will not be approved if it does not follow a modern framework. If you are using a framework that is not in this list, please contact us to find out if that framework is approved.

<table>
  <tr>
    <td>Node.JS
Hapi.js
Sails.js
Express.js
Koa.js
Mojito.js



Java
Spring
JSF
Vaadin


Scala
Lift
Play
</td>
    <td>
Meteror.js
Derby.js
Mean.js
Total.js







GWT
Grails
Play
</td>
    <td>PHP
Drupal
Cake.php
Laravel
Symfony
CodeIgniter
Yii 2


Python
Django
TurboGears
Web2py



Clojure
Luminus

</td>
    <td>
Phalcon
Zend
Slim
Fuel
PhPixie
</td>
    <td>Ruby
Ruby on Rails
Sinatra






GO
Beego
Martini
Gorilla
GoCraft


Microsoft
All Frameworks Accepted
</td>
  </tr>
</table>


** More frameworks can be supported upon request. Please contact us if your framework does not fall into this list. Front-end frameworks such as Angular, React, Backbone etc. are also approved.

## Logging

We recommend that you follow standard logging practices for your application. This should involve logging to a file on your servers. Any client side logging must be limited for the production level app as it will be shown to the user if the inspect their chrome console. Take care not to log any sensitive data.

However, you need to make sure that you are not logging excessively as you can crash Hub with an infinite loop of log statements. If your app causes performance issues or impacts performance negatively in any way, your application will be blacklisted from Hub. We will do this by disabling your application globally our production database.

## Performance

**Minification/Uglification/Concatenation**

While your app will have to be submitted in non-uglified mode, all apps running in our production environment should be minified and uglified. This increases performance and reduces initial load times. We should only ever be serving one file to the user for the client side portion of your application.

## Security

**HTTPS**

Your app must be hosted on Https to interface with Hub.

**XSS**

Your code should be protected against XSS (Cross site scripting attacks).

XSS is a type of [computer security](https://en.wikipedia.org/wiki/Computer_security) [vulnerability](https://en.wikipedia.org/wiki/Vulnerability_(computer_science)) typically found in [web applications](https://en.wikipedia.org/wiki/Web_application). XSS enables attackers to [inject](https://en.wikipedia.org/wiki/Code_injection) [client-side scripts](https://en.wikipedia.org/wiki/Client-side_script) into [web pages](https://en.wikipedia.org/wiki/Web_page) viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass [access controls](https://en.wikipedia.org/wiki/Access_control) such as the [same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy). Cross-site scripting carried out on websites accounted for roughly 84% of all security vulnerabilities.

Their effect may range from a petty nuisance to a significant security risk, depending on the sensitivity of the data handled by the vulnerable site and the nature of any security mitigation implemented by the site's owner.

(*) https://en.wikipedia.org/wiki/Cross-site_scripting

**CSRF**

Another common JavaScript security vulnerability is [Cross-Site Request Forgery](http://www.veracode.com/security/csrf) (CSRF). This is something your app should prevent.

Cross-site request forgery, also known as one-click attack or session riding and abbreviated as CSRF, is a type of malicious [exploit](https://en.wikipedia.org/wiki/Exploit_(computer_science)) of a [website](https://en.wikipedia.org/wiki/Website) where unauthorized commands are transmitted from a [user](https://en.wikipedia.org/wiki/User_(computing)) that the website trusts. Unlike [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) (XSS), which exploits the trust a user has for a particular site, CSRF exploits the trust that a site has in a user's browser.

(*) https://en.wikipedia.org/wiki/Cross-site_request_forgery

**CORS**

Where applicable, your app should have restricted CORS (Cross origin resource sharing) domains. This will prevent unwanted requests from unwanted domains. Since our Hub servers will send requests to your server, you will need to whitelist the sandbox, staging and production urls for Hub.

Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a [web page](https://en.wikipedia.org/wiki/Web_page) to be requested from another [domain](https://en.wikipedia.org/wiki/Domain_name) outside the domain from which the resource originated.

A web page may freely embed images, [stylesheets](https://en.wikipedia.org/wiki/Style_sheet_(web_development)), scripts, [iframes](https://en.wikipedia.org/wiki/Iframes), videos and some plugin content (such as [Adobe Flash](https://en.wikipedia.org/wiki/Adobe_Flash)) from any other domain. However, embedded [web fonts](https://en.wikipedia.org/wiki/Web_fonts) and [AJAX](https://en.wikipedia.org/wiki/AJAX)([XMLHttpRequest](https://en.wikipedia.org/wiki/XMLHttpRequest)) requests have traditionally been limited to accessing the same domain as the parent web page (as per the [same-origin security policy](https://en.wikipedia.org/wiki/Same-origin_policy)). "Cross-domain" AJAX requests are forbidden by default because of their ability to perform advanced requests (POST, PUT, DELETE and other types of HTTP requests, along with specifying custom [HTTP headers](https://en.wikipedia.org/wiki/HTTP_headers)) that introduce many cross-site scripting security issues.

CORS defines a way in which a browser and server can interact to determine safely whether or not to allow the cross-origin request. It allows for more freedom and functionality than purely same-origin requests, but is more secure than simply allowing all cross-origin requests. It is a recommended standard of the [W3C](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium).

(*) https://en.wikipedia.org/wiki/Cross-origin_resource_sharing

## App languages/localization

Hub will support various languages worldwide. Your app must support a base set of these languages for all strings presented to the users. Your app will also be required to listen for HTTP calls to inform your app of when the user has changed their language in Hub. Then you will be required to update your user interface to reflect the selected language.

Currently supported languages:

* English

Future Supported Languages: (Estimated date is end of Q3 2016)

* German

* Spanish

* French

* Italian

* Mandarin
