To Use Devise's Confirmable and Recoverable, an application needs to be able to send mail to users. A different approach is needed for local development compared to staging and production.

#### Devise

Setup the email address mail will come from in `config/initializers/devise.rb`:
```
config.mailer_sender = "no-reply@example.com"
```

#### Action Mailer

Setup ActionMailer settings. First in `config/environments/development.rb`:

```
# ActionMailer Config
config.action_mailer.default_url_options = { :host => 'example.dev' }

# change to true to allow email to be sent during development
config.action_mailer.perform_deliveries = false
config.action_mailer.raise_delivery_errors = true
config.action_mailer.default :charset => "utf-8"
```

Then in `config/environments/shared/remotes.rb':

```
# ActionMailer Config
# Setup for production - deliveries, no errors raised
config.action_mailer.delivery_method = :smtp
config.action_mailer.perform_deliveries = true
config.action_mailer.raise_delivery_errors = false
config.action_mailer.default :charset => "utf-8"

config.action_mailer.smtp_settings = {
  :address   => "smtp.mandrillapp.com",
  :port      => 25,
  :user_name => ENV["MANDRILL_USERNAME"],
  :password  => ENV["MANDRILL_API_KEY"]
}
```

Then in `config/environments/production.rb`:
```
config.action_mailer.default_url_options = { :host => ENV['PRODUCTION_HOST'] }
```

And in `config/environments/staging.rb`:
```
config.action_mailer.default_url_options = { :host => ENV['STAGING_HOST'] }
```

And in `config/environments/test.rb`:
```
# ActionMailer Config
config.action_mailer.default_url_options = { :host => 'example.dev' }
```
