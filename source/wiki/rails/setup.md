#### Rails Setup

Create a Rails project using Postgresql for its database and skipping the creation of Test Unit files. Name the project using lowercase words separated with hyphens.
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
```
Check remote is up and running:
```
$ heroku ps --remote staging
$ heroku open --remote staging
```

Make sure staging runs the correct environment by setting environmental variables:
```
heroku config:set RACK_ENV=staging RAILS_ENV=staging --remote staging
```

Check to see if there is a remote set up for staging:
```
$ git remote -v
```

If it isn't there, add it manually:
```
$ git remote add staging git@heroku.com:plp-s.git
```

Push local Envs
```
$ heroku config:push --remote staging
```

Set staging as the default app (to avoid adding `-a example-app-name` to every Heroku CLI command)
```
$ git config heroku.remote staging
```

Add (free) database backups.
```
heroku addons:add pgbackups --remote staging -a example-app
```

Enable compile time ENV access
```
heroku labs:enable user-env-compile -a example-app
```

Push and migrate
```
$ git push staging master
$ heroku run rake db:migrate --remote staging
```

*Repeat the above with `production`.*

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

#### Environments

Staging and Production will share many of the same config settings. To DRY this up, we create a separate class for their shared settings which both include in `config/environments/shared/remotes.rb':

```
Example::Application.configure do
  # Settings specified here will take precedence over those in config/application.rb
  # Shared settings ...
end
```

Then move all shared settings from `config/environments/production.rb` and `config/environments/staging.rb` to it, leaving both only with unique settings. Both classes need to require `config/environments/shared/remotes.rb':

Require settings shared between Production and Staging

```
require Rails.root.join('config/environments/shared/remote')

Example::Application.configure do
  # Settings specified here will take precedence over those in config/application.rb
  config.action_mailer.default_url_options = { :host => ENV['PRODUCTION_HOST'] }
end
```

#### DotEnv

Manage local environmental variables with .env.

#### Add Environmental Variables

Get the [Stationkeeping default envs](https://raw.github.com/stationkeeping/rails/master/.env) 
```
$ curl -O https://raw.github.com/stationkeeping/rails/master/.env
```

Push Environmental Variables to Production and Staging
```
heroku config:push --remote production
heroku config:push --remote staging
```

Remove `public/index.html`

#### Setup an S3 Bucket for assets

Log into the [Amazon S3 Console](https://console.aws.amazon.com/console/home):

Create a bucket using [app-name]-[tld]-assets e.g. *example-app-name-com-assets*.

Add a CORS Configuration if you need to support XHRs:

```
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

### Asset Sync


Set the asset controller host for your environment. This should match the FOG_DIRECTORY env 
```
config.action_controller.asset_host = "//pathways-learning-platform-assets.s3.amazonaws.com"
```

Add any CSS/JS assets that are not referenced from the manifests to the 
```
  # Precompile additional assets (application.js, application.css, and all non-JS/CSS are already added)
  # config.assets.precompile += %w( search.js )  
  # config.assets.precompile << 'manifests/*.js'
  # config.assets.precompile << 'manifests/*.css'
```

Make sure a digest is created:
```
  config.assets.digest = true
```