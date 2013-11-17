---
title: "Rails Settings And Startup"
---

When running a Rails app (via `runner`, `console`, `server`)

1. boot.rb sets up Bundler and load paths
2. application.rb loads rails gems, gems for the specified Rail.env, and configures the application
3. environment.rb runs all initializers

#### Application.rb 

Is required by `environment.rb` and configures the application.

It loads all the rails components using `require 'rails/all`, but this can be replaced with individual components as desired.

Anything it sets can be overridden in environment-specific files. 

Add non-default groups for use in `Gemfile`:

```
if defined?(Bundler)
  # For custom groups, define which environments they should be included in
  groups = {
    assets: %w(development test),
  }
  Bundler.require(:security, :model, :view, *Rails.groups(groups))
end
```

Add other non-standard folders to Rails's class path:

```
config.autoload_paths += %W(#{config.root}/app/models/forms)
```

Set timezone:

```
config.time_zone = "London"
```

Add folders or files for precompilation if they aren't referenced in `application.css` or `application.js`.

```
config.assets.precompile += "precompile/*.js"
```

You can also set up ActiveRecord Observers, Generators and Localisation.

##### Other settings

###### Autoload

By default, Rails performs *automatic class reloading*, meaning it reloads classes for every request, picking up any changes made. It does this by using `load` instead of `require` (which would only load a class the first time it is encountered).

To see the load path:

```
$LOAD_PATH
```

`lib` is not added to the autoload path, though it is added to the `$LOAD_PATH`. Files in `lib` need to be referenced using `require`. Or can be added to the autoload path using:

```
config.autoload_paths += %W(#{config.root}/lib/*)
```

This autoloading functionality is configured using:

```
config.cache_classes = false
```

###### Errors

Rails supplies useful error messages in local development. To display these messages on a remote machine:

```
config.consider_all_requests_local = true
```

###### Caching

By default, caching is turned off:

```
config.action_controller.perform_caching = false
```

###### Mail

Prevent mailer from raising an error if it can't send, for example if the development environment is not able to send mail:

```
config.action_mailer.raise_delivery_errors = false
```

Or prevent it even trying (mails are still logged)

```
config.action_mailer.perform_deliveries = false
```