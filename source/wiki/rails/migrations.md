---
title: "Rails Migrations"
---

To generate a migration:

```
$ rails generate migration NAME [field:type field:type] [options]
```

For example:

```
$ rails generate migration create_example name:string age:integer
```

If a migration is irreversible, it should raise an `ActiveRecord::IrreversibleMigration` in its down method.

To rollback one step:

```
$ rake db:rollback
```

To rollback a given number of steps:

```
rake db:rollback STEP=n
```

To rollback to a specific migration (using the timestamp of that migration):

```
$ rake db:rollback VERSION=20090124223305
```

To roll all the way back:

```
$ db:migrate:down
```

#### Various

`create_table` automatically adds an integer-typed primary key. 

Always use `:decimal` rather than `:float` for precision decimal numbers.

By default a column value is `null`. To set a default value:

```
default: value
```

To make a column required at a database level (adding a `not_null` constraint):

```
null: false
```


