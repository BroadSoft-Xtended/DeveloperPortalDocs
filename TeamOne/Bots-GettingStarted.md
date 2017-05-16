# Getting Started with Bots

This will get you going with the most basic Team-One bot. In this example, we will be building an app in Node.js ([download it here](https://nodejs.org/en/download/))

MacOS: You will need brew installed if you don't have all the required dependencies. `https://brew.sh/`. In this example, we will be building an app in [Node.js](https://nodejs.org/en/). We will be hosting it locally for testing.

Windows/Other: Please see the docs [here](https://medium.com/@WWWillems/how-to-install-cygwin-node-js-npm-and-webpack-for-windows-7-c061443653d3) for info on how to install nodejs.

## Important Notes

- Your app has to be running locally when you try to talk to it in Team-One.

## Steps

### 1. Create a bot in the Dev Portal

Go here and log in. https://developer.broadsoftlabs.com/#/app/login

If you don't have a Team-One account yet, don't worry, you can get one free for 30 days. If you need longer than 30 days email me at jodonnell@broadsoft.com

Once you have logged in to Team-One, go to: https://developer.broadsoftlabs.com/#/app/make and click the 'Create New Bot' button in the Team-One section. Enter a bot first name and last name and select the organization you recognize from the drop down. Then click 'Create Bot'.

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/1.png" alt="Drawing" style="width: 300px;"/>

You should now see an bot like this:

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/2.png" alt="Drawing" style="width: 600px;"/>

### 2. Run the sample code

```
git clone https://github.com/BroadSoft-Xtended/SampleApps.git
cd sample-apps/teamone/MathBot
rm -rf .git && git init && git add . && git commit -am 'init'
npm install
KEY=<yourBotToken> ORG_ID=<yourOrgId> node app.js
```

Where yourBotToken and yourOrgId are found by createing a bot in the Developer Portal. This can be done here: https://developer.broadsoftlabs.com/#/app/make

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/3.png alt="Drawing" style="width: 600px;"/>

You should now see something like this:

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/4.png" alt="Drawing" style="width: 600px;"/>

### 3. Talk to your bot in Team-One

Go to https://app.intellinote.net/

Search for the bot user you created by name.

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/5.png" alt="Drawing" style="width: 200px;"/>

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/6.png" alt="Drawing" style="width: 300px;"/>

Say '/math' to the bot.

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/7.png" alt="Drawing" style="width: 600px;"/>

Your bot is now working.

<img src="https://raw.githubusercontent.com/BroadsoftLabs/BroadsoftExternalDocs/master/TeamOne/images/8.png" alt="Drawing" style="width: 600px;"/>

## Its not working for me.

- Try restarting Team-One in your browser by reloading the page.
