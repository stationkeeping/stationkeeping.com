---
title: "Ember Templates"
---

#### Block Expressions

##### Logic

Define a property in a controller:

```
App.ApplicationController = Ember.Controller.extend({
  isOK: false
});
```

And use logic in the template:

```
{{#if isOK}}
  <h1>OK</h1>
{{/if}}
```

Or:

```
{{#if isOK}}
  <h1>OK</h1>
{{else}}
  <h1>Not OK</h1>
{{/if}}
```

Or:

```
{{#unless isOK}}
  <h1>Not OK</h1>
{{/unless}}
```

##### Enumeration

Define a property in a controller:

```
App.ApplicationController = Ember.Controller.extend({
  things: [{name:"ONE"}, {name:"TWO"}]
});
```

Then in the template:

```
<ul>
  {{#each things}}
    <li>The thing is: {{name}}</li>
  {{/each}}
</ul>
```

Or preserving scope:

```
<ul>
  {{#each thing in things}}
    <li>The thing is: {{thing.name}}</li>
  {{/each}}
</ul>
```

You can add an `else` to either of these loops:

```
<ul>
  {{#each thing in things}}
    <li>The thing is: {{thing.name}}</li>
  {{else}}
    <li>No thing at all</li>
  {{/each}}
</ul>
```

##### Setting Scope

Use `with` to set the scope. For example normally:

```
{{thing.name}}
```

But using scope:

```
{{#with thing}}
  {{name}}
{{/with}}
```

##### Binding attributes of HTML elements to controller properties

This includes classes and ids. 

```
App.ApplicationController = Ember.Controller.extend({
  logoUrl: "graphics/logo.png"
});
```

```
<div id="logo">
  <img {{bindAttr src="logoUrl"}} alt="Logo">
</div>
```

If you are using it for classes and the property is a boolean, it works as a switch:

- false = no class added
- true = dasherised version of property name added (`someProperty` becomes `some-property`)

To decouple the a boolean property name from the class, use:

```
<div {{bindAttr class="isTrue:class-to-add-if-true:class-to-add-if-false"}}>
  <p>WHatevers</p>
</div>
```

##### Actions

A basic action (by default `action` will listen for `click`):

The controller:

```
ExampleApp.ApplicationController = Ember.Controller.extend({

  doSomething: function(param) {
    console.log("Did Something "+param);
  }
});
```

In the template:

```
<button {{action "doSomething" Beautiful}}>Click Me</button>
```

Or if you want a different action:

```
<button {{action doSomething" Beautiful on="mouseUp" }}>Click Me</button>
```

And to prevent bubbling to parent:

```
<button {{action doSomething bubbles=false }}>Click Me</button>
```

