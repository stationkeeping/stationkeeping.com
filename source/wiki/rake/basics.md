---
title: "Rake Basics"
---

To show all available tasks:

```
$ rake -T
```

*Note: Tasks will not show up if they don't have a `desc`.*

Create a new task by creating a `.rake` file:

```
desc "A Description of the task"
task :example do
   # Standard ruby code to do things
end
```

To create dependencies between tasks:

```
task :dependee do
   # Do things
end

task :depender => :dependee do
   # Do things
end
```

To create multiple dependencies (no need for block):

```
task :example => [:foo, :bar]
```

*Note: Tasks are run in order*

Tasks can be placed in a namespace:

```
namespace :app 
   desc "An example task"
   task :example do
   end
end
```

Which will be available with:

```
$ rake app:example
```

Standard Ruby methods can be added to Rake files:

```
task :example do
   foo();
end

def foo()
  # Do stuff
end

```

#### Rails Specific
 
In Rails tasks should be placed in `lib/tasks`. 

To load the Rails environment, use a dependency of `:environment`:

```
task :example => :environment do
   # Do stuff
end
```