---
title: "Ember Views"
---

Mostly, a template alone will suffice, however sometimes complex event handling necessitates a View.

A View translates DOM events to semantic events meaningful to the application.

Create a view (specifying its template):

```
var view = Ember.View.create({
  templateName: 'exampleTemplate'
});
```

Attach to/Remove from document:

```
view.appendTo('#container'); // To specific element
view.append(); // To body
view.remove(); // To body
```

Insert a view into a template:

```
{{view App.ExampleView}}
```
Or with content within:
```
{{#view App.ExampleView}}
   This is a clickable area!
{{/view}}
```

