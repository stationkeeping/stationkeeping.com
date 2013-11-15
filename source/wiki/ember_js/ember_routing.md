---
title: "Ember Routing"
---

Routes are defined in the Router (roughly equivalent to Rails' `config/routes.rb`.

```
ExampleApp.Router.map(function() {
  this.route("exampleTemplate", { path: "/examplePath" });
});
```

By default, "/" is mapped to the index template. Ember automatically creates an `IndexController` if you don't. 

To customise what happens when a route is navigated to, subclass Route:

```
App.IndexRoute = Ember.Route.extend({
  
});
```

For example, to use `ExampleController` instead of `IndexController`.

```
ExampleApp.Router.map(function() {
  this.route("example", {path: "/"});
});
```


