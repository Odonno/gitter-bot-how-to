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
2. Join the room(s) that you want the bot to be activated on

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

* You will need a Heroku account in order to continue
* Install [Heroku Toolbelt](https://toolbelt.heroku.com/)

### Deployment Steps

**NOTE:** The steps regarding Keep alive, sleep and wake up are ONLY required if you are running on a Hobby Heroku instance.  If you are using a paid for plan, then you can ignore these steps.  When running on a Hobby Plan, the Heroku Dyno is only allowed to run for 18 hours a day.  This keepalive routine allows the bot to continue running without any interaction, and still remain within the rules of the free account.

1. Navigate to the directory where you created your bot above
2. `heroku login`
3. `heroku create`
4. `heroku config:set HUBOT_GITTER2_TOKEN=****` (here the token is the Personal Access Token for Github Account that will be running as the bot.
5. `heroku config:set HEROKU_URL=https://<URL>` (this is to keep the heroku application alive.  The URL is generated from the `heroku create` command above
6. `heroku config:set HUBOT_ADAPTER="gitter2"` (this ensures we use the gitter2 adapter)
7. `heroku config:set HUBOT_HEROKU_KEEPALIVE_INTERVAL=5` (this is the interval that the heroku instance will be polled to keep it active.  Value is in minutes)
8. `heroku config:set HUBOT_HEROKU_KEEPALIVE_URL=https://<URL>/` (this is URL that will be pinged each keep alive attempt.  Should be the same as the URL above.  Notice the trailing slash, which is very important!)
9. `heroku config:set HUBOT_HEROKU_SLEEP_TIME=22:00` (this is UTC time at which the hubot instance will go to sleep.  22:00 is the default, but you can change this to whatever suits you.)
10. `heroku config:set HUBOT_HEROKU_WAKEUP_TIME=06:00` (this it the UTC time at which the hubot instance will wake up.  06:00 is the default, but you can change this to whatever suits you.)
11. `heroku addons:create scheduler:standard` (this adds the free heroku scheduler to your account)
12. Sign into your heroku account, and click on the newly created scheduler
![image](https://cloud.githubusercontent.com/assets/1271146/13035913/173d6322-d352-11e5-931f-4dbf73f45e4b.png)

13. Edit the settings of the job to look like the following:
![image](https://cloud.githubusercontent.com/assets/1271146/13035919/4791916a-d352-11e5-8f0c-ca9561f540ed.png)
**NOTE:** The next due time you be set as the same as the `HUBOT_HEROKU_WAKEUP_TIME` above.
14. `git push heroku master`
15. `heroku logs` (if all goes well here, you should see something simalar to the following)

![image](https://cloud.githubusercontent.com/assets/1271146/5890975/1b0b13d4-a471-11e4-97db-9be2b5fbae77.png)

## Host on Azure

### Prerequisites

* You will need an Azure account in order to continue.
* Install Azure Command Line Interface - `npm install -g azure-cli`

### Configuration Steps

**HOTFIX:** Due to the fact that there has been a fix on the `develop` branch of the `hubot-gitter2` repository to make it work on Azure, you will need to make the following adjustment to the `package.json` file. Change this line :

`"hubot-gitter2": "git://github.com/huafu/hubot-gitter2.git#develop",`

Once a new release of the `hubot-gitter2` is available, you will not need to do this change.  You can subscribe to [this issue](https://github.com/huafu/hubot-gitter2/issues/18) to know when a new release is available.
 
1. `azure site deploymentscript --node`
2. Edit `external-scripts.json` file and remove these lines
    * `"hubot-heroku-keepalive",`
    * `"hubot-redis-brain,`
3. Create a new file `server.js` in the root directory that contains these 2 lines
    * `require('coffee-script/register');` <br />
      `module.exports = require('bin/hubot.coffee');`
4. `npm install coffee-script --save`
5. Open `deploy.cmd` and add a new line under `Deployment` section (after the 3rd step)
    * `:: 4. Create Hubot file with a coffee extension` <br />
      `copy /Y "%DEPLOYMENT_TARGET%\bin\hubot" "%DEPLOYMENT_TARGET%\bin\hubot.coffee"`
6. Commit this change on your repo (`git commit -m "Add Azure deployment configuration"`)
7. Publish your code to your favorite source control (see below in Steps.3)

### Deployment Steps

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

### Keep alive

If you use the free version of Azure websites, you could be suprised that your bot stop to after around 20 minutes. That's because the application pool sleep after a period of inactivity. To keep your service awake, you can use the service [uptimerobot.com](http://uptimerobot.com/).

1. Sign-up or login to the service
2. Add new monitor
3. Configure your monitor like this
    * Monitor type : `Ping`
    * Friendly Name : `<yourbotname>`
    * IP (or host) : `http://<yourbotname>.azurewebsites.net/hubot/help`
    * Monitoring interval : every 15 minutes

# Ready to use

If all of the above has worked, go to your Gitter Chat Room, and try issuing a hubot command like `hubot ping` and hopefully you will see the following:

![image](https://cloud.githubusercontent.com/assets/1271146/5890979/96fa7066-a471-11e4-9042-b1db63b4e984.png)

# Credits

* Thanks to [CultureOps](https://cultureops.wordpress.com/) for his blog post [*Running Hubot in an Azure website ?*](https://cultureops.wordpress.com/2015/06/03/running-hubot-in-an-azure-website/)
