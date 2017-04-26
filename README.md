# Developer Portal Docs

You can edit these docs with a pull request. This can be done by [editing the files directly](https://help.github.com/articles/editing-files-in-your-repository/) in github. Create a [pull request](https://help.github.com/articles/creating-a-pull-request/) this way and I will review and merge it.

Updating the docs database.

## Step 1 Clear the database

mongo ds153765.mlab.com:53765/html -u `<dbuser>` -p `<dbpassword>`

In the mongo shell:

use html;
db.dropDatabase();

## Step 2 Populate DB

Do a get request to : https://markdown-converter.broadsoftlabs.com/initDatabase/BroadSoft-Xtended/DeveloperPortalDocs
