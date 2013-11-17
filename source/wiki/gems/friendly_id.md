---
title: "Friendly ID"
---

Friendly ID replaces ids in URLs with a human-readable string

Create Table to track slug history:

```
$ rails g friendly_id
```

Add functionality to Models, where `:example` is an attribute or method:

```
extend FriendlyId
friendly_id :example, use: [:slugged, :history]
```

And add to table creation migration:

```
t.string :slug 
```

Or as a separate migration:

```
add_column :examples, :slug, :string
```

And add index:

```
add_index :examples, :slug, , unique: true
``
