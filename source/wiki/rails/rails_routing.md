---
title: "Rails Routing"
---

1. Map URL to Controller
2. Genrate URLS (via URL helpers)

When matching a route, the rooter starts from the top of `routes.rb` and works its way down, checking each route for a match until it finds one. If it reaches the end without finding a match, it raises an exception.

Basic match (all HTTP verbs):

```
match "example/foo" => "example#foo"
```

Match a particular HTTP verb:

```
get "example/foo" => "example#foo"
post "example/foo" => "example#foo"
```

Or for multiple HTTP verbs on the same route:

```
match "example/foo" => "example#foo", [via: :get]
```

Adding a segment key is like setting the segment as a variable: `:example` means that the value of this segment of the url will be set as a parameter, with a key being the segment key:

```
get "example/foo/:example" # url of example/foo/66 would generate a parameter of params[:example] = 66
```

##### Redirects

You can redirect a path directly using:

```
match "/foo", :to => redirect("/bar")
```

Or using a block (with optional status):

```
match "/foo/:id", :to =>
  redirect(status: 302) {|params| "/bar/#{params[:id]}" }
```

##### Constraints

Using constraints allows a segment key, anything in the params hash, or a number of other values like `subdomain` and `referrer` to be validated before a route matches. By using a second unconstrained route, any routes that failed to validate can be handled:

```
match ':controller/show/:id' => :show, :constraints => {:id => /\d+/} #Check id is numerical
match ':controller/show/:id' => :show_error
```

Or constraining multiple routes:

```
constraints :subdomain => '' do
   …
end
```

To access the `request` object in constraints:

```
match 'example/:id' => "example#show",
  :constraints => proc {|request| request.params[:id].to_i < 100 }
```

##### Root Route

The root of your domain is routed using:

```
root :to => "pages#show"
```

##### Root Globbing

To expose an arbitrary number of segments to a controller as variables, effectively saying that 'after this point in the url, pass everything else in as a variable':

```
get "example/foo/*remaining => example#show
```

So anything in the url after `foo` will be added to the `params` hash as :remaining:

```
example/foo/some/thing # params[:remaining] = "some/thing"
example/foo/some/thing/else/longer # params[:remaining] = "some/thing/else/longer"
```

##### Named Routes

Routes can be exposed via URL helpers using `as`. To generate a helper `bar_path`:

```
get "example/foo" => example, as: 'bar'
```

To pass variables into a URL helper:

```
example_path(id: example_model.id)
```

Or if it's a numerical id:

```
example_path(example_model.id)
```

Or use the object itself:

```
example_path(example_model)
```

##### Displaying text instead of id

Rails calls `to_param` to get the variable value to insert into the URL. By default it returns the model's `id`, but by overriding `to_param` anything can be returned, for example, the model name. Anything returned should be suitable for use in a URL:

```
def to_param
  name.parameterize
end
```

The flip side of using a slug rather than an id is that when a controller finds the model using the URL segment, it needs to do so using the appropriate attribute, for example:

```
Example.find_by_parametised_name(params[:slug])
```

#### Resources

Restful routes can be auto-generated in `routes.config` by using:

```
resources :example
```

Or for singular resources (which gives all routes other than `index`:

```
resource :example
```

Additional routes can be added using:

```
resources :example do
  get :foo, on: :member
  get :bar, on: :collection
end
```

Or for multiple HTTP verbs for the same route:

```
resources :example do
  match :foo, via: [:get, :post], on: :member
  match :bar, via: [:get, :post], on: :collection
end
```

To map custom names to the standard actions:

```
resources :example, :path_names => { :new => 'foo', :edit => 'bar'}
```

##### Nested Routes

Resources can be nested using: 

```
resources :foo do
  resources :bar
end
```

