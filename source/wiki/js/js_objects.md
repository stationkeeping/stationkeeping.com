---
title: "JS Objects"
---

#### New Object

```
var object = new Object();
object.example = "example";
```

Is the same as:

```
var object = {example: "example"};
```

#### Call and this

To set the scope of a function (what `this` refers to inside the function), call can be used. In this example, an anonymous object is passed as the first parameter to call, so exampleFunction's `this` points to the anonymous object.

```
var exampleFunction = function() {
   console.log(this.name);
}

exampleFunction.call( {name: "example"} );
```

#### Constructors 

A constructor is just another function, but conventionally its name is upper case:

```
function Example(name) {
   this.name = name;
   this.value = 33;
}
```

By then calling `new Example(name)`, the constructor's `this` property is set to a new Object instance which is automatically returned by the constructor function.

*The object will also have a reference to the constructor function used to create it in its `constructor` property*

A constructor is just that - a constructor. It is constructing an instance. Any functions created in the constructor are created very time the constructor is run. Conversely, any functions assigned using prototype are added to the prototype a single time (but don't have access to nay local variables in the constructor).

#### Adding functions

Although functions can be added to the constructed object in the constructor function, these are added on a per-instance basis - they are recreated and added for each instance that is created. This is wasteful in terms of memory. The alternative is to set them on the object's prototype. This is done once and the functions will be available to all objects:

```
Example.proptotype.exampleFunction = function() {
}
```

#### Prototype Chaining (Like Subclassing)

The constructor of the chained object is called, using `call` to set the scope of the chained constructor to the derived constructor. Any parameters it needs are also passed in. This is very similar to a call to `super`. The chained constructor function runs and adds any properties or functions to the object created by the derived constructor. We also need to manually set the constructor to the derived constructor as it will be set to the chained object's constructor when it is created using `new`.

```
function DerivedExample(name) {
   Example.call(this, name);
   this.constructor = DerivedExample;
}
```

We also need to link the prototype of the chained class to the derived class, so that the derived class gains access to the functions on the chained class:

```
DerivedExample.prototype = new Example();
```
