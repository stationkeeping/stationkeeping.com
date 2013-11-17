Generate a migration. This will create two tables: `tags` and `taggings`. Tags have a polymorphic `has_many` relationship with their models, allowing tags to be used with many different types of model:
```
rails g acts_as_taggable_on:migration
```

To add tagging to a model:
```
class Example < ActiveRecord::Base
  attr_accessible :tag_list
  acts_as_taggable
end
```
The attribute `tag_list` is part of the gem and will return an array of tag names. The tags themselves can be accessed through `tags`.

The tags are of type: `ActsAsTaggableOn::Tag` with the following attributes:
```
id: integer
name: string
```