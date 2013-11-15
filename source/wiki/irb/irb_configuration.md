---
title: "IRB Configuration"
---

The IRB equivalent of `.bash_rc` / `.bash_profile` is `~/.irbrc`.

Example [file](https://github.com/logankoester/irbrc/blob/master/irbrc). 

```
require 'rubygems'


require 'ap' # Awesome Print
require 'net-http-spy' # Print information about any HTTP requests being made

# ASCII table views
require 'hirb/import_object'
Hirb.enable
extend Hirb::Console

# Enable coloured output
require 'wirble'
Wirble.init
Wirble.colorize

# Bash-like tab completion
require 'bond'; Bond.start
```