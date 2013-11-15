---
title: "Rails Polymorphic Associations"
---

Polymorphic associations allow a single model to belong to multiple models of different classes *via the same association* rather than it maintaining a separate association to each type of model.

#### has_many

Two different models that both need to be rateable, `One` and `Two`:

```
class One < ActiveRecord::Base
  has_many :ratings, :as => :rateable
end

class Two < ActiveRecord::Base
  has_many :ratings, :as => :rateable
end
```

And a single `Rating` model:

```
class Rating < ActiveRecord::Base
  belongs_to :rateable, polymorphic: true
end
```

The models that need to share an association class declare that they are `polymorphic` using `as` along with the polymorphic name:

```
has_many :ratings, :as => :rateable
```

And the model that they are sharing declares a single `belongs_to` association, with the same polymorphic name:

```
belongs_to :rateable, polymorphic: true
```

To support this association in the underlying tables, on the model class shared between the polymorphic models, two columns are needed. The first; `rateable` stores the `id` of whichever polymorphic `rateable` model satisfies the association. The second column; `rateable_type` specifies the class of the model. Both can be generated using:

```
class CreateRatings < ActiveRecord::Migration
  def self.up
    create_table :comments do |t|
      t.references :rateable, :polymorphic => true
   end

   add_index :comments, [:commentable_id, :commentable_type]
end
```

#### has_many through

Two different models that both need to be rateable through rating_assignment, `One` and `Two`:

```
class One < ActiveRecord::Base
  has_many :rating_assignments, :as => :rateable
  has_many :ratings, through :rating_assignments
end

class Two < ActiveRecord::Base
  has_many :rating_assignments, :as => :rateable
  has_many :ratings, through :rating_assignments
end
```

A Rating Assignment model:

```
class RatingAssignment
   belongs_to :rateable, polymorphic: true
   belongs_to :rateable
end
```

And a single `Rating` model:

```
class Rating < ActiveRecord::Base
  has_many :rating_assignments
  has_many :ones, through: rating_assignments, source: :rateable, source_type: "One"
  has_many :twos, through: rating_assignments, source: :rateable, source_type: "Two"
end
```

