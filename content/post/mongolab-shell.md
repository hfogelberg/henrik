+++
date = "2015-11-18T18:12:41+01:00"
title = "Connecting to Mongolab From the Shell"
menu="main"
+++
For some maintenance work on a Meteor site running on Heroku I needed to connect to the database from the shell. It is of course possible, but finding out how to do it required a bit of digging in the documentation and an email to Mongolab’s support, They responded quickly (thank you!) and here it goes:

## 1. Finding your credentials
To find your password and user id cd in to the root of the repo you are working on and run this command: `$ heroku config | grep MONGO`.

The response will look something like this
````
MONGOLAB_URI:    mongodb://heroku_user_id:some_random_password@dc729610.mongolab.com:55862/heroku_user_id
````

Your user credentials are between the slashes and the @-sign, so:
````
user id: heroku_user_id
password: some_random_password
````

## 2. Logging in
Logging in from the shell is now easy
````
$mongo dc729610.mongolab.com:55862/heroku_user_id -u heroku_user_id -p some_random_password
````

## 3. Trouble shooting
If you can’t log in the two most likely reasons are:
- Network issues
- Credentials

To check if you have network issues (perhaps something is blocked in the fire wall?) you can in fact connect to Mongolab without your username and password. You can for obvious reasons not do anything when you are connected, but you can at least find out if you can access the database. 
````
$mongo dc729610.mongolab.com:55862/heroku_user_id
````

If that works, recheck your credentials!

