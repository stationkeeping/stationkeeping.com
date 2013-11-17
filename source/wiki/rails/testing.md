---
title: "Testing"
---

Gemfile setup:

```
# Used in generators
group :development, :test do
  gem 'rspec-rails'
  # Fixture Factory
  gem 'factory_girl_rails'
end

group :test do
  gem 'faker'
  gem 'capybara'
  gem 'guard-rspec'
  gem 'launchy'
end
```

Install RSpec:
```
$ rails g rspec:install
```

Make Capybara available across tests but including it in `spec/spec_helper.rb:
```
require "capybara/rspec" # Adds browser integration
```

Tell RSpec to use the 'documentation' output format by adding to `.rspec`:

```
--format documentation
```

Tell Rails to generate specs for newly created classes by adding to `config/application.rb:

```
config.generators do |g| 
   g.test_framework :rspec, 
      :fixtures => true, 
      :view_specs => false, 
      :helper_specs => true, 
      :routing_specs => true,
      :controller_specs => true,
      :request_specs => true 
   g.fixture_replacement :factory_girl, 
     :dir => "spec/factories" 
end
```

Prepare the test database:
```
$ db:test:prepare
```

Run specs:
```
$ rspec spec/models/example_spec.rb
```

#### Mocking and Stubbing

- Stub - Stub out a method so it returns a value you define, eliminating side-effects
- Mock - Intercept the method call (allowing us to test if it is called and what was passed to it)

To test if a method is called, we can use:
```
@example_model.should_receive(:example_method)
@example_model.method_triggering_example_method
```
Now, the test will pass if the call to `method_triggering_example_method` triggers a call to `example_method`.

If we wanted to check on the value passed to the mocked method:
```
@example_model.should_receive(:example_method).with('Example Value')
```

#### RSPEC

The `describe` block is the most general block. It can be used to describe a thing.

A Class:
```
describe ExampleClass
```
A function:
```
describe "example_function"
```

A "context" block is functionally equivalent (context is aliased to describe), however it's use is different. It should be used to denote a particular state or context:
```
context "save was not successful"
context "user_license_expired"
```

The "it" block defines a test.

#### Testing Models

Create a factory for each model in `spec/factories' named as a plural of the model. The factory should configure a default object with all its attributes set to validating values. This valid model can then be used to test failed validations by setting single attributes to invalid values.

Create a spec for the model in `spec/models`.

1. Sanity check that the model's factory generates a valid model
2. Check that missing in invalid values for the model's attributes cause the model to be invalid
3. Check methods on the model
4. Check scopes on the model (checking order)

#### Testing Controllers

Create a spec in `specs/controllers`.

Inside your Controller's describe block create a further describe block for each action, eg:
```
describe "GET #index" do 
   it "populates an array of examples" 
   it "renders the :index view" 
end
```

1. Test assigns are correctly set
2. Test view is rendered (or browser redirected)

If the controller is for a nested resource, make sure you also pass parent id(s) in.