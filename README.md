# Monitoring Celery with Flower on Heroku

[Flower](https://flower.readthedocs.io/en/latest/) is a great tool for monitoring [Celery](https://docs.celeryproject.org/en/stable/django/first-steps-with-django.html) processes but sadly cannot be deployed in the same instance as your primary [Heroku application](https://heroku.com). A simple solution is to run Flower on a seperate Heroku instance. This simple project will launch Flower with Redis to monitor your Celery processes from another project.

It's so simple, we can do it in only a few easy steps:

## Step 1 - Get the code!

Clone this repo!

`git clone https://github.com/paqman85/simple-celery-flower-on-heroku.git`

## Step 2 - Give it a home! Create a new Heroku application

Create a Heroku app for Flower:

### On the command line:

1. Login to Herku:

`heroku login`

2. Create a new app for Flower:

`heroku create YOUR-DESIRED-APP-NAME-HERE`

### On Heroku's website

1. Login to your Heroku account

2. Create a new application instance from your dashboard

## Step 3 - Make the Roots! Set your Broker url.

Flower needs to conenct to your Celery broker url in order to monitor your Celery Processes. This project includes Redis as a default - so feel free to use your Redis or RabbitMQ broker url.

### On the Command Line:

`heroku config:set BROKER_URL=redis://... -a YOUR-APP_NAME`

### On the Heroku Website:

1. While in your application's dashboard, click on the settings tab.
2. Click *reveal vars* button in the Config Vars section
3. Add a new key and value -- the key is `BROKER_URL` and the value is the url to your Celery broker for the application you want to monitor... if redis it would start with `redis://`

## Step 4 - Lock The Door! Add some Authentication

The project assumes you want to keep things simple and use Basic Authentication. We simple need to add the username and password to the environment variables.

### On the Command Line:

`heroku config:set FLOWER_BASIC_AUTH="username:password" -a YOUR-APP_NAME`

### On the Heroku Website:

1. Add a new key and value -- the key is `FLOWER_BASIC_AUTH` and the value is the username and password you want to use to login to Flower. `username:password`

## Step 5 - Deploy!

It's time to deploy! 

### On the Command Line:

If you don't have git set up yet:
`git init`
`git status`
`git add . `
`git commit -m "Name your commit`

Then set heroku as remote:

`heroku git:remote -a YOUR-APP_NAME`

And here is the command to push to heroku:

`git heroku push master`

You can confirm all is working well by checking `heroku logs --tail -a YOUR-APP_NAME'`

## Step 6 - Monitor That Celery!

Now if everything worked out - you should be able to login to your application at your heroku app url and monitor your Celery processes!
