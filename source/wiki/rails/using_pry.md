---
title: "Rails Using Pry"
---

Notes from Screencast: http://pryrepl.org/

Install:

```
$ gem install pry pry-doc --no-ri --no-rdoc
```

Run:

```
$ pry
```

Add a semi-colon to a method call to redact the return value from the STDout:

```
$ example;
```

Tab completion comes out of the box.

Within Pry:

`$ reset` Reset the REPL to a clean state.
`$ whereami` Show code surrounding the current context.
`$ cd` Move into a new context (object or scope).
`$ ls` Show the list of vars and methods in the current scope.

Pry can be configured using a `.pryrc` file in the user directory.



The [Pry Rails Gem](https://github.com/rweng/pry-rails) gets Pry working with the Rails console, so that `$ rails c` opens Pry instead of IRB.

It adds a `show-routes` command that is equivalent to `$ rake routes`
