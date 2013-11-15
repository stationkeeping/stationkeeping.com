---
title: "Ruby Gems Basics"
---

By default gems are installed into `~.bundle`. You can tell bundler to use a project directory instead using:

```
$ bundle install vendor
```

This will sue symlinks to a single location. To install actual gems locally:

```
bundle install vendor --disable-shared-gems
```

In this case bundler  iterm  

If a gem name is different from the name needed in a require statement, for example the gem is called `sqlite3-ruby` and the name is `sqlite`, the gem will not be auto-required, and you would need to add `require  sqlite` to use it. 

To map the gem name to the correct name so it is auto-required, use:

```
gem "sqlite-rails", require: "sqlite"
```

For using a gem from a Git repo:

```
gem 'paperclip', :git => 'git://github.com/thoughtbot/paperclip.git'
```

For using a local gem:

```
gem 'example', :path => '~/code/example'
```

For using a local gem locally only, and a git repo remotely:

```
$ bundle config.example "Path/to/local/git/repo/of/example
```

Then add the gem to the `Gemfile` as a git repo instead of a Rubygem. The `github` argument can be blank:

```
gem "example",  github: "", branch: "master"
```

To install gems without installing gems from particular groups, for example on production:
```
$ bundle install --without development test
```

To package gems to `vendor/cache` to avoid install from external sources:

```
$ bundle package
```

This could be used to deploy gems from a local repo to a remote location that doesn't have access to the local repo.

Rbenv installs gems to: 

```
{USER_HOME}/.rbenv/versions/{RUBY_VERSION}/lib/ruby/gems/{COMPATABILITY_LEVEL}/gems/{GEM_NAME}-{GEM_VERSION}
```

For example:

```
/Users/me/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/example-1.15.0
```