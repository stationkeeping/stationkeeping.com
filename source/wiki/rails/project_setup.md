---
title: "Rails Project Setup"
---

#### Rails Setup

Create a Rails project using Postgresql for its database and skipping the creation of Test Unit files
```
$ rails new example-name -d postgresql -T
```

Change directory:
```
$ cd example-name
```

Add the [Stationkeeping Gemfile](https://github.com/stationkeeping/rails/blob/master/Gemfile) as base and *remove what you don't need*.
```
curl -O https://raw.github.com/stationkeeping/rails/master/Gemfile
```

Then install the Gems using:
```
$ bundle install
```

Fix the Ruby version:
```
$ rbenv local 1.9.3-p194
```

Drop the folder onto Sublime Text and once it's open choose: `Project > Save Project As …`, saving the project inside the new Rails project folder with the same name as the project.

Add [Stationkeeping .gitignore](https://github.com/stationkeeping/rails/blob/master/.gitignore).
```
curl -O https://raw.github.com/stationkeeping/rails/master/.gitignore
```

Add staging environment by copying `config/environments/production.rb`:
```
$ cp -i config/environments/production.rb config/environments/staging.rb
```
*Now modify `staging.rb` as required.*

Add a `README` file:
```
$ touch README.md
```

Initialise a new repository:

```
$ git init
```

Add project to version control:

```
$ git add -A
$ git commit -m "Initial commit. Added gems to Gemfile and project-specific gitignore"
```

Create a new Github or Bitbucket Repository. Add it to your local repository as a remote and push it.

Setup POW symlink to make application available on `.dev` using:
```
$ powder link
```

####Heroku Setup

Create Heroku app for `staging`:
```
$ heroku create example-name --remote staging --region eu
$ git push staging master
$ heroku run rake db:migrate --remote staging
```
Check remote is up and running:
```
$ heroku ps --remote staging
$ heroku open --remote staging
```
*Repeat the above with `production`.*

Make sure staging runs the correct environment by setting environmental variables:
```
heroku config:set RACK_ENV=staging RAILS_ENV=staging --remote staging
```

Add (free) database backups to production:
```
heroku addons:add pgbackups --remote production
```

#### Database Setup

Open `config/database.yml` and change usernames from application name (default) to PostgreSQL username. Also delete production.

```
common: &common
  adapter: postgresql
  encoding: unicode
  username: pedr

development:
  <<: *common
  database: pathways-archive_development

test:
  <<: *common
  database: pathways-archive_test
```

#### Config

Add the following to `/config/application.rb`:

```
config.autoload_paths += %W(#{config.root}/lib)
config.assets.initialize_on_precompile = false
```

#### DotEnv

Load Environmental Vars immediately so they are available to Rails as it boots by adding the following to `/config/boot.rb`

```
# Load Environmental variables
require 'dotenv'
Dotenv.load
```

Remove `public/index.html`