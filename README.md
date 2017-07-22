
# App-Innovation-Workshop - Exercises

# 1 - Setup on Cloud9
- Create a Cloud9 Account
- Fork the Github repo (https://github.com/samuelsharaf/app-innovation-workshop) to your github account.
- On Cloud9 console clone the repo e.g. if you are cloning from heroku/app-innovation-workshop you will use the following command:
```
git clone https://github.com/samuelsharaf/app-innovation-workshop
```
- Once the cloud9 repo is created, Run this command:
  ```
  sh setup.sh && . ~/.profile
  ```

  
# 2 - Deploy to Heroku
From the command-line in Cloud9 run the following commands:
```
heroku login
heroku apps:create
git push heroku master
```
Open your Heroku app and you should see the app running

# 3 - Monitoring: Exercise
## Offense
You are a hacker. You live in eastern hackistan. You have a contract with EvilCorp to take down a site that competes with them. Your mission is to take down WellCo!
The discovery phase of your attack has already been completed. A colleague of yours has identified several HTTP endpoints that seem to have weaknesses:

- `/messages`
- `/clocks`
- `/rockets`
- `/beverages`
- `/maps`
- `/pipes`
- `/bins`
- `/handles`
- `/children`

Your task is to pick one of these endpoints, and attack it with everything you have. Try to take down WellCo:

```
artillery quick --duration 60 --rate 10 -n 20 http://my.app.dev/api/resource
```

If and when the WellCo engineer can identify the endpoint you are attacking, they will block it, and your attack will be thwarted.
## Defense
You are a dev ops engineer at WellCo - a company that makes products that work well.
You are on call, supporting an app that has been deployed to production.
Unfortunately, this app has gained the reputation for being "the problem app". And now it's 23:30 on a Friday, and you get an SMS message.

The "problem app" is acting up again. You suspect the site may even be under denial-of-service attack.

But how do you identify the vulnerability? Use your knowledge of Logplex and NewRelic to decide which endpoint is being attacked!  Once you've determined that, try to form a hypothesis as to which resource is being used up by the endpoint by inspecting New Relic and Logplex.

## Turn About is Fair Play
After you have successfully attacked and defended an end-point, switch roles! WellCo should attack EvilCorp.
Pick a different endpoint, and work your way through until you can identify the signature of each attack, using Logplex and NewRelic.
Happy hacking!

# 4 - Scaling

- https://devcenter.heroku.com/articles/scaling
- https://devcenter.heroku.com/articles/node-concurrency

## Exercise
- Run an `artillery` command
- Observe the `rps` (requests per second)
- Scale the app out  (more dynos)
- Re-run the `artillery` command
- Observe the `rps` (requests per second) is higher

Don't forget to scale down at the end of the day!

# 5 - Logging
- https://devcenter.heroku.com/articles/papertrail
- https://devcenter.heroku.com/articles/logentries
Hit one of the error endpoints, setup triggers, and watch what happens in the log aggregators!

# Heroku CI/CD and Review Apps
## Setup the Pipeline
- Open the app
- Go to "Deploy"
- Click "New Pipeline..."
- Click "Create Pipeline"
- Click on the menu for the app and select "Move app to `staging`"

## Setup CI
- Click on "Tests"
- Click on "Connect to Github"
- Search for heroku-workshop
- Click "Connect"
- Click "Enable Heroku CI"
- Click the "Tests" tab
- Click "+ New Test"
- Click "Start Test Run"
You should see the test pass.

## Continuous Deployment
- Click on the app menu for the staging app
- Click "Configure Automatic Deploys"
- Check "Wait for CI to pass before deploy"
- Click "Enable Automatic Deploys"

## Review Apps
- Click "Enable Review Apps..."
- Check "Create New..." and check "Destroy stale..."
- Click "Enable"

## View a Review App in Action
- In Github go to your "Code" tab of your repo
- Click "New Pull Request"
- In the first dropdown choose your fork
- The dropdowns will change
- In the first dropdown choose "master"
- In the second dropdown choose `pr-1-tasks-list`
- You should see 3 changed files
- Click "Create Pull Request"
- In Heroku you should see that a new Review App will be created

  > NOTE: Tests will _not_ run because for this demo you haven't created a new commit.  The tests would have been run when this branch was pushed in real life.

- Open the URL and make sure things look good
- Go to Github and merge the pull request
- The tests should pass and it should auto-deploy

And that's a real-life review-app -> CI -> CD scenario!

## Viewing an error
- Create a new pull request from master -> pr-2-task-create-test-broken
  > This branch contains a test that will pass

- You'll see that it creates a review app - you can ignore this
- Manually trigger a test build

  > NOTE: in production developers would be pushing code, and these would all be automatic.  But for this workshop we don't want you to write a lot of code...

- Merge this broken code to master
- You'll see that the staging app is _not_ rolled in
- Merge pr-3-task-create-works into master (this fixes it)
- You'll see that staging gets auto-deployed

# Integrating with Sendgrid

Sendgrid is a 3rd-Party Service that's integrated with Heroku.  Getting setup with Sendgrid couldn't be easier:

- Add the Sendgrid addon

  ```
  heroku addons:create sendgrid:starter
  ```

- Login to Sendgrid
- Create an api key at https://app.sendgrid.com/settings/api_keys
- Set it on your app with `heroku config:set SENDGRID_API_KEY="..."`
- Set the `FROM_EMAIL` environment variable `heroku config:set FROM_EMAIL="..."`
- Create a task in your app
-





