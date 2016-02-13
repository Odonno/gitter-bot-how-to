# gitter-bot-how-to [![Join the chat at https://gitter.im/Odonno/gitter-bot-how-to](https://badges.gitter.im/Odonno/gitter-bot-how-to.svg)](https://gitter.im/Odonno/gitter-bot-how-to?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A tutorial on how to make a Gitter Bot

# Prerequisites

* **A GitHub account** to post to your Gitter rooms. While you could use your own account, this may be confusing to your users, so a new, separate account may be better
* Once you have created your new GitHub account, log into the [Gitter Developer Page](https://developer.gitter.im/docs/welcome), and then access this [page](https://developer.gitter.im/apps) and take a note of the accounts **Personal Access Token**
* You are going to need a **cloud infrastructure** somewhere to host your hubot instance. For example, you can host on [Heroku](https://dashboard.heroku.com/) or on [Azure](https://azure.microsoft.com/), or on your own
* Have node.js installed (which includes npm)

# First steps

To get hubot, these are the steps that should be followed.

**NOTE:** Although there are two hubot adapter's for Gitter, we found that only one of them works reliably. Namely, this [one](https://github.com/huafu/hubot-gitter2). The other [one](https://github.com/kcjpop/hubot-gitter) seems older, and has been replaced by the one that we ended up using.

1. Login into [Gitter](http://gitter.im) with the GitHub account that you are going to use as your bot
1. Join the room(s) that you want the bot to be activated on

# Configure your bot

You can follow the installation process from hubot-gitter2 [here](https://github.com/huafu/hubot-gitter2#installation).

Or you can follow these steps :

1. `npm install -g hubot coffee-script yo generator-hubot`
2. `mkdir -p <yourbotname>` where `<yourbotname>` is the name of your bot
3. `yo hubot` (when prompted, enter `gitter2` as adapter name, and `<yourbotname>` as name
4. `npm install --save hubot-gitter2`
5. Create git repository (`git init`, `git add .`,  `git commit -m "Initial commit"`)

**NOTE:** Ideally, you would then want to push the repository that you just created to "somewhere" for storage, perhaps GitHub or BitBucket.

You can then test your bot with the following command line where `<your token>` is the token provided by Gitter.

`HUBOT_GITTER2_TOKEN=<your token> ./bin/hubot -a gitter2`

# Publish

## Host on Heroku

### Prerequisites

* Install [Heroku Toolbelt](https://toolbelt.heroku.com/)

### Steps

1. Navigate to the directory where you created your bot above
2. `heroku login`
3. `heroku create`
4. `heroku config:set HUBOT_GITTER2_TOKEN=****` (here the token is the Personal Access Token for Github Account that will be running as the bot.
5. `heroku config:set HEROKU_URL=https://<URL>` (this is to keep the heroku application alive.  The URL is generated from the `heroku create` command above
6. `heroku config:set HUBOT_ADAPTER="gitter2"` (this ensures we use the gitter2 adapter)
7. `git push heroku master`
8. `heroku logs` (if all goes well here, you should see something simalar to the following)

![image](https://cloud.githubusercontent.com/assets/1271146/5890975/1b0b13d4-a471-11e4-97db-9be2b5fbae77.png)

If all of the above has worked, go to your Gitter Chat Room, and try issuing a hubot command like `hubot ping` and hopefully you will see the following:

![image](https://cloud.githubusercontent.com/assets/1271146/5890979/96fa7066-a471-11e4-9042-b1db63b4e984.png)

## Host on Azure

### Prerequisites

You'll need an Azure account to continue.

### Steps

1. Login into [Azure dashboard](https://portal.azure.com/)
2. Create a new **App Services** with the name, for example `<yourbotname>`
3. Under the **Publishing** settings, click on **Continuous deployment** section and then choose the source of your code among these options :
    * Visual Studio Team Services
    * OneDrive
    * Local Git Repository
    * GitHub
    * Bitbucket
    * Dropbox
    * An external repository
4. Under the **General** settings, click on **Application settings** and fill with key/value pairs
    * add gitter token (key: `HUBOT_GITTER2_TOKEN`, value: `<your token>`)
    * add hubot adapter (key: `HUBOT_ADAPTER`, value: `gitter2`)