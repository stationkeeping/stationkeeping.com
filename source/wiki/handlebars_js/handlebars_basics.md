---
title: "Handlebars Basics"
---

#### Basic

Example of a formatting:

```
<h1>{{title}}</h1>
```

1. Make the template available on the page using a script tag:

```
<script id="title-template" type="text/x-handlebars-template">
  <h1>{{title}}</h1>
</script>
```

2. Compile it to JavaScript using:

```
var source   = $("#title-template").html();
var template = Handlebars.compile(source);
```

3. Execute and populate using:

```
var context = {title: "Example Title"}
var html    = template(context);
```

#### Helpers

Register a helper:

```
Handlebars.registerHelper('capitalize', function(title) {
  return title.capitalize();
});
```

Then Use it:

```
<h1>{{capitalize title}}</h1>
```