---
layout: post
title:  "Deploying your SQLite3 Sinatra App to Heroku using PostgreSQL"
date:   2017-06-24 22:14:11 +0000
---


Hello everyone!  I had quite the learning experience today and I thought I should share it with all of you!

The Problem: 

Here at the Flatiron School we mainly use SQLite3 for our database needs.  This is great for us as students because SQLite is very easy to setup and manage.. but not so great for Heroku deployment because it isn't supported! This means we need to use a database tool such as PostgreSQL to get our app up and running on Heroku.  In this post, I'll walk you through what it takes to get your SQLite3 Sinatra project switched over to PostgreSQL for Heroku!

The Solution:

To create a Heroku account, head over to www.heroku.com!  The site will guide you through the initial setup, so I'll start by assuming you've got heroku ready to go on your computer.

Deploying to Heroku is easy.  Just enter the following commands:

```
heroku create *your-project-name-here*
git push heroku master
```

UH OH.  Failed?  Yep, failed.  Thank SQLite3 for that!

To fix this, let's get PostgreSQL enabled for production!  When your app is up and running on Heroku, it is running in a "production" environment (as opposed to development or test).  Fortunately, this actually allows us to use BOTH SQLite3 AND PostgreSQL with our app!  This way you can keep managing and testing your app locally just like you always have with SQLite3 but let PostgreSQL do all the heavy lifting on Heroku!

The first thing we need to do is edit our Gemfile.  I would start by removing "gem 'sqlite3'" from the main list of gems and then adding the following code to the bottom:

```
group :development do
   gem 'sqlite3'
end

group :production do
   gem 'pg'
   gem 'activerecord-postgresql-adapter'
end
```

The 'pg' gem is PostgresQL.  Now we need to dive into our "config" directory.  If you don't have one already, create a file called 'database.yml' in the 'config' directory (which should be where you environment.rb file is).
Put the following code in your new 'database.yml' file:

```
development:
  adapter: sqlite3
  database: database.db

test:
  adapter: sqlite3
  database: database.db

production:
  adapter: postgresql
  database: sch_database #rename this to whatever you want
  username: username
  password: password
  host: localhost
```

Now to the enviroment.rb file.  You'll need to REMOVE anything that looks like:

```
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)
```

and replace it with:

```
configure :development do
  ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)
end

configure :production do
  db = URI.parse(ENV['HEROKU_POSTGRESQL_COBALT_URL'] || 'postgres://localhost/mydb')

  ActiveRecord::Base.establish_connection(
    :adapter  => db.scheme == 'postgres' ? 'postgresql' : db.scheme,
    :host     => db.host,
    :username => db.user,
    :password => db.password,
    :database => db.path[1..-1],
    :encoding => 'utf8'
  )
end
```

Save your progress and take a deep breath.  We're almost there.  Now we need to install an addon to our heroku app.  Do this at the following link: https://addons.heroku.com/heroku-postgresql . You can select the free database plan and select your application. Heroku then gives you a path name for your database. This will likely be: HEROKU_POSTGRESQL_COBALT_URL .  You should notice this URL in the code above. On Heroku, navigate to YourApp > Settings > click "Reveal Config Vars" to double check, if this doesn't work.

Your Heroku application should now recognize the database.

Now redeploy your code to Heroku,

```
git add .
git commit -m "message about Heroku deployment here"
git push origin master
git push heroku master
```

and this time there should be no errors associated with SQLite3!

Now all we need to do is run our migrations with:

```
heroku rake db:migrate
```

and seed our brand new PostgreSQL database with:

```
heroku rake db:seed
```

AND WE'RE DONE!  If everything went smoothly, you should be able to navigate to *your-app*.herokuapp.com and use your newly deployed app!

I hope this little guide has helped ease the pains of getting your Sinatra app running on Heroku.  Thanks for reading and happy coding! 

<3 Luke


