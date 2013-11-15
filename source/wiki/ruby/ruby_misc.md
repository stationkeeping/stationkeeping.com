---
title: "Ruby Misc"
---

Ruby stores the list of paths it will search for code in `$`.

Show paths Ruby is using:

```
$ puts $
```

Paths can be added using the env `RUBYLIB`.

Execute ruby code from command-line using the `-e` flag:

```
$ ruby -e "puts 'example'"
```