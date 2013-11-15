---
title: "Debugging Using Console"
---

Open/close with `Alt / Cmd / I`

##### Console

Use assertions in the source to check application state:

```
console.assert(1 == 9); // Raises Exception
```

Inspect an object using `inspect`:

```
$ inspect(exampleObject)
```

Get a reference to the last selected element in the DOM using `$0` and previous items with `$1`, `$2`, `$3` etc.

##### Breaking on Exceptions

Sources Tab > Console

Pause button has three modes:

- [Blue] Break on any exception
- [Purple] Break on uncaught exception
- [Black] Don't break

##### Opening Debugger At Point In Code

Add `debugger;` at the point in your code you want to debug. When the execution hits that line the application will break.

##### Look at an object's structure from the console 

Some objects, like classes only show up as strings, but you can inspect the object properly using:

```
$ dir(someClass)
```

Shift-click refresh to reload the page, bypassing the cache.