---
title: "Rails Debugging Errors And Exceptions"
---

**Don't use minified vendor CSS/JS files. They are often missing important debug messages and using a debugger is unpleasant. When you precompile your assets *they will all be minifed anyway*.**

#### Better Errors
Better Errors gives much more useful error pages, allowing you to drill down through the stack, jumping to the source file and embeds a console so you can investigate the application live.

Use `better_errors` and `binding_of_caller` gems:

```
gem 'better_errors'
gem 'binding_of_caller'
```

To see the last exception: `{base_url}__better_errors`

To customise the editor that is opened from Better Errors, add a `better_errors.rb` initialiser:

```
BetterErrors.editor = :sublime if defined? BetterErrors
```

#### Rails Panel
Rails Panel adds a new tab to Chrome Developer Tools called *Rails*, that displays both HTML and JavaScript requests within your Rails app.

You need to add the *Meta Request* gem to your Gemfile:

```
gem 'meta_request'
```

And install the Chrome Extension.

To set Sublime Text as the editor linked to the Plugin, choose Sublime from: `Chrome > Prefs > Extensions > Rails Panel` and install `[SublimeText 2 URLHandler](https://github.com/asuth/subl-handler)' which enables support for `subl://` URLs.

Check Speed of gem load (loads and runs gist found [here](https://gist.github.com/panthomakos/2588879):

```
bundle exec ruby -e "$(curl -fsSL https://gist.github.com/raw/2588879/benchmark.rb)" | sort -n -k4
```