---
title: "Ruby Metaprogramming"
---

There are no such things as class methods in Ruby. Classes are objects and every method is a method on an object. 

#### Eigenclass

A method can be defined on a single object (instance):

```
class Test
end 

a = Test.new
b = Test.new

def a.method_unique_to_this_instance
   puts 'Unique'
end

a.unique # Unique
b.unique # NoMethodError

```

In this situation, an anonymous *Eigenclass* is created to house the singleton method and inserted as the class of `a`, with its superclass pointing at its original class. This class is also hidden from us. Calling `a.class` returns its original superclass.

This means that methods added to a *class* are also done so using anonymous classes because they are added to the shared class object using `self`:

```
class Test

   def self.some_class_method
   end

   def Test.some_other_class_method
   end

end
```

In both these situations, an Eigenclass is created housing these methods and set to `Test`'s parent class.
