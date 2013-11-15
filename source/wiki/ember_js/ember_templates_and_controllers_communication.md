---
title: "Ember Templates And Controllers Communication"
---

##### Display Controller Property

Define a property in a controller:

```
App.ApplicationController = Ember.Controller.extend({
  examplePropery: "Example"
});
```

And reference it in a template:

```
<p>{{exampleProperty}}</p>
```

##### Automatically Update Property from Input (`action` defaults to the Return Key here, or Click for other views).

```
{{view Ember.TextField valueBinding="exampleProperty"}}
```

##### Update the same property, but call a function on the controller whenever it is changed:

```
{{view Ember.TextField valueBinding="exampleProperty" action="exampleFunction"}}
```

##### Call a function in the Controller whenever a property is changed:

```
ExampleApp.ApplicationController = Ember.Controller.extend({

  exampleProperty: "Example",

  exampleFunction: function() {
    console.log("Example property was changed to: "+this.get("exampleProperty");
  }.observes('exampleProperty')
  
});
```