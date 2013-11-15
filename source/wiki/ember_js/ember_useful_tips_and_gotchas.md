---
title: "Ember Useful Tips And Gotchas"
---

[List of DOM events](http://emberjs.com/guides/understanding-ember/the-view-layer/#toc_adding-new-events)

See all the Templates Ember knows about in the application: 

```
$ Ember.TEMPLATES
```

Set the template used by the application:

```
ExampleApp.ApplicationView = Ember.View.extend({
  templateName: 'news_items/application'
});
```

Set the element to append the app template to:

```
ExampleApp = Ember.Application.create({
  rootElement: $('#example-container')
});
```

Tell Ember how to correctly pluralize a Model:

*In `store.js`*

```
DS.RESTAdapter.configure('plurals', {example:'examples'});
```

Bind the disabled property of a button to the number of items in an ArrayController, so that if there are no items, the button is disabled:

```
<button {{action clearAll}} {{bindAttr disabled="anyEntries"}}>Clear All</button>
```

```
  anyEntries: function() {
    return this.content.get('length') == 0;
  }.property('content.length'),
```

*I'm creating new models, but the view isn't updating, even when I refresh. However the models are there.*

This is probably an issue with the Route. It isn't passing the Models to the COntroller correctly.