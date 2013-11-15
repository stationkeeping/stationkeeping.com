---
title: "Angular Basics"
---

#### Client-Side Templates

Clear division between server and browser. Data and static assets from server. Populates templates and displayed in the browser

### Basics

#### MVC

- Views: DOM Nodes
- Controllers: JavaScript classes
- Model: Object properties

#### Dependency Injection

Declare what a class needs and it will be supplied by the container.

#### Directives

HTML is extended using directives (like components).

### More

#### Controllers

Scope Controllers to the app module:

Unscoped:

```
function AppController($scope){};
```

Scoped:

```
module.controller("AppController", function($scope) {});
```

Controllers have three responsibilities in your app:

- Set up the initial state in your application’s model
- Expose model and functions to the view (UI template) through $scope
- Watch other parts of the model for changes and take action

Responsibilities:

- Should never reference the DOM
- Should handle view behaviour - Decide what should happen if user does X and decide where to get Y from

#### Services

Services are Singletons.

Responsibilities:

- Should (almost) never reference the DOM
- Contain logic independent of the view

#### Directives

Responsibilities

- Watch DOM
- Manipulate DOM

### Scope

- Scope should be read-only in templates; a template should only ever read from scope.
- Scope should be write-only in controllers; a controller should only ever write to scope.