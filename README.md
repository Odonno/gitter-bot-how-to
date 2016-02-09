# gitter-bot-how-to [![Join the chat at https://gitter.im/Odonno/gitter-bot-how-to](https://badges.gitter.im/Odonno/gitter-bot-how-to.svg)](https://gitter.im/Odonno/gitter-bot-how-to?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A tutorial on how to make a Gitter Bot

# Prerequisites

* **A GitHub account** to post to your Gitter rooms. While you could use your own account, this may be confusing to your users, so a new, separate account may be better
* Once you have created your new GitHub account, log into the [Developer Gitter](https://developer.gitter.im/docs/welcome) page, and then access this [page](https://developer.gitter.im/apps) and take a note of the accounts **Personal Access Token**
* You are going to need a **cloud infrastructure** somewhere to host your hubot instance. For example, you can host on [Heroku](https://dashboard.heroku.com/) or on [Azure](https://azure.microsoft.com/), or on your own

# Setup

To get hubot, these are the steps that had to be followed.

**NOTE:** Although there are two hubot adapter's for Gitter, we found that only one of them works.  Namely, this [one](https://github.com/huafu/hubot-gitter2).  The other [one](https://github.com/kcjpop/hubot-gitter) seems older, and has been replaced by the one that we ended up using.

* Login into [Gitter](http://gitter.im) with the GitHub account that you want to run as your bot
* Join the room that you want the bot to be activated on
* Install node.js (which includes npm)
* Install Heroku Toolkit
* `mkdir C:\heroku\<yourbotname>`
* `cd .\<yourbotname>`
* `heroku login`
* `npm install --global coffee-script hubot`
* `npm install --global yo generator-hubot`
* `yo hubot` (when prompted, enter `gitter2` as adapter name, and `<yourbotname>` as name
* `npm install --save hubot-gitter2`
* `git init`
* `git add .`
* `git commit -m "Initial commit"`
* `heroku create`
* `heroku config:set HUBOT_GITTER2_TOKEN=****` (here the token is the Personal Access Token for Github Account that will be running as the bot.
* `heroku config:set HEROKU_URL=https://<URL>` (this is to keep the heroku application alive.  The URL is generated from the `heroku create` command above
* `heroku config:set HUBOT_ADAPTER="gitter2"` (this ensures we use the gitter2 adapter)
* `git push heroku master`
* `heroku logs` (if all goes well here, you should see something simalar to the following)

![image](https://cloud.githubusercontent.com/assets/1271146/5890975/1b0b13d4-a471-11e4-97db-9be2b5fbae77.png)

If all of the above has worked, go to your Gitter Chat Room, and try issuing a hubot command like `hubot ping` and hopefully you will see the following:

![image](https://cloud.githubusercontent.com/assets/1271146/5890979/96fa7066-a471-11e4-9042-b1db63b4e984.png)
