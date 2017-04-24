# Getting Started with Bots

This will get you going with the most basic Team-One bot. These instructions are for MacOS. You will need brew installed if you don't have all the required dependencies. `https://brew.sh/`. In this example, we will be building an app in [Node.js](https://nodejs.org/en/). We will be hosting it on locally for testing.

## Important Notes

- Your app has to be running locally when you try to talk to it in Team-One.

## Steps

### 1. Create a bot in the Dev Portal

Go here and log in. https://developer.broadsoftlabs.com/#/app/login

If you don't have a Team-One account yet, don't worry, you can get one free for 30 days. If you need longer than 30 days email me at jodonnell@broadsoft.com

Once you have logged in to Team-One, go to: https://developer.broadsoftlabs.com/#/app/make and click the 'Create New Bot' button in the Team-One section. Enter a bot first name and last name and select the organization you recognize from the drop down. Then click 'Create Bot'.

<img src="https://puu.sh/ugmiU/38e8f24701.png" alt="Drawing" style="width: 300px;"/>

You should now see an bot like this:

<img src="https://puu.sh/ugmB9/636593da25.png" alt="Drawing" style="width: 600px;"/>

### 2. Run the sample code

```
git clone https://github.com/BroadSoft-Xtended/sample-apps.git
cd sample-apps/teamone/TeamOne-MathBot
rm -rf .git && git init && git add . && git commit -am 'init'
npm install
KEY=<yourApiKey> ORG_ID=<yourOrgId> node app.js
```

You should now see something like this:

<img src="https://puu.sh/ugmsw/61f2996c74.png" alt="Drawing" style="width: 600px;"/>

### 3. Talk to your bot in Team-One

Go to https://app.intellinote.net/

Search for the bot user you created by name.

<img src="https://puu.sh/ugmDk/6de8156a19.png" alt="Drawing" style="width: 200px;"/>

<img src="https://puu.sh/ugmFI/8bba0cfa3e.png" alt="Drawing" style="width: 300px;"/>

Say '/math' to the bot.

<img src="https://puu.sh/ugmLx/7d870138a6.png" alt="Drawing" style="width: 600px;"/>

Your bot is now working.

<img src="https://puu.sh/ugnVw/f614affa60.png" alt="Drawing" style="width: 600px;"/>

## Its not working for me.

- Try restarting Team-One in your browser by reloading the page.
