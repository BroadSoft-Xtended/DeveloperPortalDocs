# BroadsoftExternalDocs

Hosted on heroku

How to update this:

## Step 1 Clear the database

mongo ds153765.mlab.com:53765/html -u `<dbuser>` -p `<dbpassword>`

In the mongo shell:

use html;
db.dropDatabase();

## Step 2 Populate DB

Do a get request to : https://markdown-converter.broadsoftlabs.com/initDatabase/BroadsoftLabs/BroadsoftExternalDocs
