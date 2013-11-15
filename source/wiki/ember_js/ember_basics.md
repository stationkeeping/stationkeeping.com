---
title: "Ember Basics"
---

#### Setup

Add `gem "ember-rails"` gem to Gemfile

Generate a new project using:

`rails g ember:bootstrap`

Specify Ember varient in each environment file:

In `config/environments/development.rb` and `config/environments/test.rb`:

`config.ember.variant = :development` 

In `config/environments/production.rb`:

`config.ember.variant = :production` 

##### Generated JavaScript

Ember adds the following to your `assets/javascripts/application.js`:

```
//= require handlebars
//= require ember
//= require ember-data
//= require_self
//= require demo_app
DemoApp = Ember.Application.create();
```

The `demo_app` file is also generated and looks like this:

```
//= require ./store
//= require_tree ./models
//= require_tree ./controllers
//= require_tree ./views
//= require_tree ./helpers
//= require_tree ./templates
//= require ./router
//= require_tree ./routes
//= require_self
```

#### Entry Point

Application's entry point, found in `application.js` is:

```
App = Ember.Application.create();
```

1. The application passes events from the document to your views
2. The application renders the application (root) template
3. The application creates a router and uses the current URL to setup initial routing.
