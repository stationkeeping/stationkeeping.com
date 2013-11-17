---
title: "Creating A Gem"
---

Generate the project:

```
$ cd to/folder/that/will/contain/gem
$ bundle gem example
```

Update the generated `example.gemspec`, specifically `authors`, `email`, `homepage` `summary` and `description`.

Add dependencies:
```
gem.add_dependency "some_gem"
gem.add_development_dependency "rake"
gem.add_development_dependency "rspec"
```

Create a `specs` folder for tests:
```
$ mkdir specs
```

Set up rake task for tests by adding the following to `Rakefile`:

```
require "rspec/core/rake_task"

RSpec::Core::RakeTask.new

task :default => :spec
task :test => :spec
```

Tests can now be run using:
```
$ rake
```

Add gem code to `lib/example.rb`. 
Any related files should go in `lib/example`

Install the gem locally:
```
$ rake install
```

Submit to rubygems:
```
$ rake release
```

#### To Include Rake Tasks

[Good Reference](http://blog.nathanhumbert.com/2010/02/rails-3-loading-rake-tasks-from-gem.html)

To include Rake Tasks in the gem that are installed and usable in a Rails project that uses the Gem, create a Railtie:

```
# lib/example/railtie.rb
require 'example'
require 'rails'
module MyPlugin
  class Railtie < Rails::Railtie
    railtie_name :example
    rake_tasks do
      load "tasks/example.rake"
    end
  end
end
```

Then load it:

```
# lib/example
module Example
  require 'example/railtie' if defined?(Rails)
end
```



