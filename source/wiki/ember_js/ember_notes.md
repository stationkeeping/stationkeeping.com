---
title: "Ember Notes"
---

Partial uses the context (view, controller) from where it was rendered from.

**If your UI is nested, Your routes should be nested**

If routes are nested, `{{outlet}}` can be used in a parent template to render the nested template.

Ember.Controller looks for data on itself
Ember.ObjectController proxies its Model, looking for data on the Model
Ember.ArrayController proxies a collection

Controllers are Singletons, accessible using `this.controllerFor()' E.g. `this.controllerFor('example')' would get `ExampleController`.

Model attribute types: string, number, boolean, date.

Start off with fixtures, then swap out for server implementation

App.ApplicationRoute = Ember.Route.Extend({
   setupController: function() {
      this.controllerFor('example').set('model', App.Example.find());
   }
}); 

`{{render}}` is designed to work with a singleton controller. It creates a new context, with its own controller, model and template.

Use for / else in templates to react to an empty array:

```
{{ #each }}

{{ else }}

{{/ each}}
```