---
title: "Setup"
---

#### Install / Setup

Install Devise:
```
$ rails g devise:install
```

Setup Email (See separate page)

Generate Devise User:
```
$ rails g devise User
```

Filter passwords from Logs in `config/application.rb`:
```
config.filter_parameters += [:password, :password_confirmation]
```

Migrate:
```
$ rake db:migrate
```

#### Set up custom views

In `config/initializers/devise.rb` tell Devise to use views specific to your Model(s):
```
config.scoped_views = true
```

Then generate the views:
```
$ rails generate devise:views
```

You'll probably want to add a class of `horizontal-form` to `config/initializers/simple_form.rb`.
```
config.form_class = 'simple_form form-horizontal'
```

Check for user authentication using `authenticate_user!` in controllers:
```
before_filter :authenticate_user!
```

#### Roles with Rolify

```
$ rails generate rolify:role
```

Set up a default role for your user in `User.rb`:
```
before_create :assign_default_role

def assign_default_role
  self.add_role :guest if self.roles.first.nil?
end
```

Add roles to a user using:

```
user.add_role :admin
```

#### Authorization with CanCan

Ryan Bates's [Cancan](http://railscasts.com/episodes/192-authorization-with-cancan?autoplay=true) separates Athorization (what a user is allowed to do) from Authentication (who the user is) and user role (admin, guest etc.).

Generate an `Ability` model:
```
￼$ rails generate cancan:ability
```

Check user authentication using `authorize!` where the format is:
```
authorize! {action}, {model}, :message => 'Example Message'

```
For example:
```
authorize! :index, @user, :message => 'Not authorized as an administrator.'
```

Handle exceptions in the `ApplicationController` by showing a Flash Message :
```
class ApplicationController < ActionController::Base
  rescue_from CanCan::AccessDenied do |exception|
    redirect_to root_url, :alert => exception.message
  end
end
```

#### Validations

Validate user attributes:
```
validates_presence_of :name
validates_uniqueness_of :name, :email, :case_sensitive => false
```