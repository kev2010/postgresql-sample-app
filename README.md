# URL Shortener
This sample app is to showcase how you would set up your application with a PostgreSQL database using the URL shortener app from Recitation 6.

## New packages
pg-promise -- for postgres (http://vitaly-t.github.io/pg-promise/)  
dotenv -- for environment variables (https://www.npmjs.com/package/dotenv)  
(See package.json)

## Installing PostGreSQL locally
Notes before starting download and install:
- At some point you will be asked to select components to download, you can include all the suggestions. You should at least include pgAdmin. pgAdmin will be the UI that you can use to interact with your DB locally.
- Note down the password for your super user.
- Allow default port setting and others.

To download and install the most recent version of postgresql, go to https://www.enterprisedb.com/downloads/postgres-postgresql-downloads.

## Starting a server
There should be a default server and database when you start the pgAdmin. By clicking on the default server under "Servers" the server should start.

You can also create a new server if need be.
- Right click on Servers and choose to create a Server.
In the General Tab
- Set name for it in the General tab
In the Connection tab
- Set Host name/address to localhost.
- Set the port number to 5432(might be there by default).
- Set the username and password to the user name and password used when setting up pg. Username is by default postgres.
- Click Save

After tables are created by your app, you can see the tables by navigatng the left section. 
Servers -> [your server] -> Databases -> [your database] -> Schemas -> Tables
Right click a table and choose View/Edit Data

## App
For your app you need to remember the following information from postgres (can be found in pgAdmin):
- username (this is postgres by default)
- password
- host (this is normally set to localhost)
- port (his is normally 5432)
- name of database (this is postgres by default)

You will need a connection detail string which is of the form 
postgres://[USERNAME]:[PASSWORD]@[HOST]:[PORT]/[DATABASE_NAME]

The main changes are in db_config.js. We will be using a new package: pg-promise. You need to require the new package and instantiate it differently(see file for more details).
Also the methods for the pgDB are different so pay attention to the changes in run, get and all functions. pg-promise already uses promises so we are no longer wrapping the calls in a promise like we did for sqlite3.

Things to note:
- In your models, correct your sql statement syntax to postgresql syntax. eg: "==" is not valid. must be "=".
This doc will be helpful: https://www.tutorialspoint.com/postgresql/postgresql_syntax.htm

## Heroku
- You need to also include the dotenv package. This allows you to set variable in a .env file and access the vairables in your application through the process.env variable. Heroku uses the .env to tell your app where the postgresql server is (see db_config.js for usage).

You should add .env to your .gitignore if it's not already there (i.e. it shouldn't be pushed), as heroku will create its own .env. It should include the following definitions (with values specific to your own database):  
DB_USER=postgres  
DB_PASSWORD=password  
DB_HOST=localhost  
DB_PORT=5432  
DB_DATABASE=postgres  

Set up Heroku
- Install the Heroku CLI if you haven't already.
- run "heroku login" and follow steps.

Create an Heroku app and include the postgresql add on.
- run "heroku create [my app name]"
- run "heroku addons:create heroku-postgresql:hobby-dev --app=[my app name]"
- run "heroku addons" to verify that your app shows up with a heroku-postgresql plan. The hobby-dev plan is free!
- run "heroku local web" to test the heroku instance locally at http://localhost:5000

Push app and changes to Heroku
- run "heroku git:remote -a [my app name]" to create your heroku remote
- push code to heroku with "git add .", "git commit -m '[commit message]'", "git push heroku main" (main or master depending on your branch name)
- you can test you app at your deployed link.

Helpful links 
- taniarascia.com/node-express-postgresql-heroku/#deploy-app-to-heroku
- https://devcenter.heroku.com/articles/heroku-postgresql#set-up-postgres-on-windows

Notes:
- this command can be used to reset your database. YOU WILL LOOSE YOUR DATA! "heroku pg:reset DATABASE"
- use "heroku logs -t" to get server side logs, this will include console.logs you add to your express app.
