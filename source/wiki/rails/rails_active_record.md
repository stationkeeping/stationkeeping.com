---
title: "Rails Active Record"
---

An Active Record instance is an object wrapping a database row. It uses the row's `id` (integer) attribute as the primary key. Rails uses the class name to infer the table name.

List tables

```
ActiveRecord::Base.connection.tables
```

List database columns:

```
Example.column_names
```

#### Macros / DSL Declarations

Macros are just bracketless method invocations called on the class object, and add functionality to it at runtime:

```
has_many :examples
has_many :foo, through: :bar
```

Is analogous to:

```
has_many(:examples)
has_many(:foo, through: :bar)
```

#### Creation

Create an ActiveRecord:

```
example = Example.new(foo: 33, bar: "something")
```

Check whether it has ever been saved:

```
example.persisted?
```

Or opposite:

```
example.new_record?
```

To create and save in one hit:

```
example = Example.create!(foo: 33, bar: "something")
```

Both `create` and `new` take an optional block which will be executed after attributes have been set:

```
example = Example.create!(foo: 33, bar: "something") do
   # Other configuration
end
```

To return an object if it exists or make a new one it if it doesn't:

```
Example.find_or_initialize_by(name:"John")
```

Or to create (new and save):

```
Example.find_or_create_by(name:"John")
```

#### Find

`first`, `last` and `all` are wrappers around `find`. If a single value is passed in it will be used to match an id, unless it is an Array, in which case each value will be used to match an id.

##### Dynamic Finder Methods

Rails catches any missing methods using Ruby's `method_missing` callback and checks the method name to see if it starts with `find_by` or `find_all_by`. If so, it uses the rest of the method to determine what to search for, for example `find_by_name`. Multiple values can be used by adding an `and`, for example: `find_by_name_and_age`. *These dynamic finder methods have been deprecated in Rails 4 in favour of `where(name:"Smith", age:77)`*

##### Where

Where returns a chainable `ActiveRecordAssociation`.

To find an object with a matching attribute:

```
Example.where(colour: "red")
```

Or for multiple:

```
Example.where(colour: "red", "blue", "green")
```

A string form is also available:

```
Example.where("colour = :colour", colour: "red")
Example.where("colour = :colour OR name = :name", colour: "red", name: "John")
```

Which allows indirect matching:

```
Example.where("colour LIKE :colour", colour: "red")
```

##### Boolean Values

Ruby will convert boolean values to the appropriate database representation.d

#### Accessors

Attributes can be read or written using dot notation, object notation (hard brackets), or ActiveRecord supports accessors to manage access to its attributes and actual read and write are done via `read_attribute(:example)` and `write_attribute(:example)`. Accessors can be overridden:

```
def example
  read_attribute(:example)
end

def example=(new_value)
  write_attribute(:example, new_value)
end
```

All attributes can be read or written as a hash of key value pairs using `attributes`. Attribute values will not be compiled through their getters, but setting attributes will pass the values through their setters.

#### Updating

For a single object:

```
Example.update(id, name: "John", age: 77)
```

For multiple objects:

```
ids = [1,2]
attributes = [{name:"John", age:77}, {name:"Jeff", age: 44}]
Example.update(ids, attributes)
```

The object(s) will always be validated, and an object will always be returned, even if it couldn't be saved and is invalid. To avoid having to check whether the object is valid again, use:

```
if object.update_attributes(name: "John")
   # Object was valid
end
```

There is also a singular form `update_attribute`, but values set using it will not be validated.

#### Saving

Using `save` will return true or false depending on whether the save was successful. Using `save!` will raise an exception if it fails.

#### Convenience Updaters

Boolean attributes can be toggled (bypassing setter) using:

```
object.toggle(active)
```

Numeric attributes can be incremented or decremented using:

```
object.increment(age)
object.decrement(age)
```

##### Update updated_at timestamp

To update the `updated_at` timestamp to now, without firing callbacks or validations:

```
object.touch
```

A `belongs_to` association will automatically call `touch` on its owner if `touch` is declared on the association:

```
class Bar < ActiveRecord::Base
  belongs_to :foo, touch: true
end
```

#### Destroy

An `ActiveRecord` instance can be destroyed using:

```
object.destroy
```

One or more records can be deleted using:

```
Example.destroy(1)
Example.destroy([1, 2, 3])
```

For a quicker destroy, but one that deletes the record directly without loading it as an `ActiveRecord` instance and therefore doesn't fire any `before_destroy` callbacks or delete any dependant associations:

```
Example.delete(1)
Example.delete([1, 2, 3])
```

#### Ordering

`order` takes a SQL fragment:

```
Example.order('created_at desc')
```

#### Limiting

Limit the maximum number of items returned by a query

```
Example.limit(10)
```

#### Offsetting

Offset the starting query (useful for paging):

```
Example.limit(10).offset(10)
```

#### Includes (Eliminating N+1 queries for associations)

If an `ActiveRecord` instance has associations, they can be loaded more efficiently by being included in the same request using `includes`:

```
class Foo
   has_many :bars
ene

>> foos = Foo.where(value:33).includes(:bars)
```

For second degree associations:

```
class Foo
   has_many :bars
ene

class Bar
   has_many :amps
ene

>> foos = Foo.where(value:33).includes(bars: [:amps])
```

#### Checking if an object exists

To get a Boolean back instead of an object:

```
Example.where(age: 22).exists?
```

#### Database Locking

##### Optimistic Locking

Optimistic Locking deals with collisions after the fact and is a good solution for situations where collisions will be infrequent. To implement optimistic locking, all a record needs a is column called `lock_version`(integer). Now, in the event that more than one client edits the record simultaneously, the first to save will win, with any subsequent saves generating an `ActiveRecord::StaleObjectError`. 

The check on the `lock_version` slows down transactions a bit, and a user isn't informed of the error until after the point at which they have already lost the data.


##### Pessimistic Locking

Pessimistic locking deals with problems before the fact by preventing a second client from editing the same row(s) in the first place. The danger of this is that something untoward happens and a lock is left in place, preventing anyone from editing the record.
