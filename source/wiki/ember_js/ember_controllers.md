---
title: "Ember Controllers"
---

Controllers make data available to Templates (and Views). Controllers Can Wrap (Decorate) Models and/or maintain their own properties. **If a template requests a property from the Controller that it doesn't offer, it will return the property of the same name on the Model.** 

You set the Model on the controller by setting its `content` property from the Router:

```
ExampleApp.ExampleRoute = Ember.Route.extend({
  setupController: function(controller, exampleModel) {
    controller.set('content', exampleModel);
  }
});
```

A Controller can alter the value of a property on the Model by implementing a function with the same property name - a getter:

```
ExampleApp.ExampleController = Ember.ObjectController.extend({
  exampleProperty: function() {
    var exampleProperty = this.get('content.exampleProperty'),
    return exampleProperty.capitalize();
  }.property('content.exampleProperty')
});
```

##### Enumerated Models

Extend `Ember.ArrayController` instead of `Ember.ObjectController` for a Controller that decorates multiple Models.

Set its content property from the Router as with an `Ember.ObjectController` subclass.

Templates can now use the controller itself as an enumeration:

```
  <ul>
    {{#each controller}}
      <li>{{exampleProperty}}</li>
    {{/each}}
  </ul>
```