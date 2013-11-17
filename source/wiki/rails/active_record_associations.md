---
title: "Rails Active Record Associations"
---

Associations are exposed as methods, and behind the scenes an association is implemented as an `AssociationProxy`. The proxy delegates most instance methods to the actual association (a ruby object or Array). `AssociationProxy` is actually the base class for a class specific to the association, with a:

```
AssociationProxy
   - BelongsToAssociation
   - HasOneAssociation
   - BelongsToPolymorphicAssociation
   - AssociationCollection 
      - HasAndBelongsToMany
      - HasManyAssociation
      - HasManyThroughAssociation
      - HasOneThroughAssociation
```

#### Indexes

Add indexes to any foreign key columns in a migration:

```
add_index :example, :foo_id
```

##### Counter Cache

Getting the number of items in a relationship is usually an expensive database operation, needing multiple hits:

```
owner.belongings.length
```

A counter cache tracks the number of items in a belongs_to relationship, ensuring the value is available on the owning instance. It is declared on the belonging member:

```
belongs_to :foo, counter_cache: true
```

And a column must be added to the owning member with a default value of 1:

```
add_column :owners, :belongers_count, :integer, default: 0
```

It is used on the owning menber:

```
owner.belongings_count
```

##### Using Associations Named Differently To The Class THey Refer To

Sometimes it may be necessary to name an association differently, for example, a relationship might be named `latest_foo`, but refer to `foo`. In this case, the `class_name` can be set:

```
has_one :latest_foo, class_name: :foo
```

It's OK to have both a `has_many` and a `has_one` to the same model, sharing a primary key:

```
class Foo
   has_many :bars
   has_one :latest_bar
end
```

##### 
