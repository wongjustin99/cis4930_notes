---
layout: post
title: "Prototype"
description: ""
category: notes
tags: [prototype]
---
{% include JB/setup %}

# Administrivia

* Exam 1 next Tuesday night 

# Prototype (117)

## Intent

Specifies the kinds of objects to create using a prototypical instance,
and creates new objects by copying the prototype. 

## Aside: What's a "First class object"? 

Simple definition: something that can be passed as an argument to a
function. 

## Initial Comments

In a language which treats classes as *first-class objects* -- e.g.,
Smalltalk & Java -- the `Prototype` pattern is competing with object
construction via a `class` object. 

The way to distinguish these, is to look at the space in which they are
competing. 

Prototype says: 

> "You're the kind of thing I want. `Clone` yourself. "

Reflection says: 

> "You represent the `Class` of thing I want. Make me an instance, yo!"

In the case where all you want is an instance of a class, it doesn't
make much difference -- from the programmer's perspective -- whether we
clone a `prototype` or instruct a `class` object to instantiate the
appropriate object. 

So..  when *would* one have cause to choose `Prototype`? 

When ANY OF THE FOLLOWING: 

1. the desired object's initial state is known to differ from the
   existing object [see *boilerplate* -- coming soon]
2. the initial state is determined at runtime [thus not known at compile
   time]
3. the initialisation process is expensive: copying an existing [and
   thus "paid for"] object is cheaper

## Structure 

![UML Diagram of
Prototype](http://silversoft.net/docs/dp/hires/Pictures/proto018.gif
"Prototype")

The Client's `operation()` is: `p = prototype.clone()`

`ConcretePrototypeA` returns a copy of self. 

## Implementation

### How to handle cloning an object that has references/pointers? 

#### Deep Copies versus Shallow Copies

A *shallow* copy has the same attribute *values* that the original has

* Thus can share data with the original

![Diagram of Prototypical & Shallow copies](http://i.imgur.com/GFuAMqs.jpg "Prototype & Shallow copies")

A *deep copy* doesn't just get values, but copies of objects that are
referenced/pointed-to: 

![Diagram of Deep Copy](http://i.imgur.com/ofRXcIp.jpg "Deep Copy")

Obviously, making *deep copies* is going to be "more expensive", both in
terms of 

* time (to make the clone)
* space (to store the non-shared data)

When an *attribute object* [i.e., an object that serves as the value of
another object's attribute] is *immutable*, then sharing is desirable.
*Mutable* attributes should be shared only with caution. 

#### `Cloneable` and `clone()` in Java

``` java
class Foo implements Cloneable {
  public Foo clone() {
    try {
      Foo fooClone = (Foo) super.clone(); // shallow copy :)

      // when appropriate, replicate selected attributes with clones

      return fooClone;
    } catch ( CloneNotSupportedException e ) {
    // this cannot happen! We support Cloneable
    throw new InternalError( e.toString() );
    }
  }

}
```

#### Don't do this: it is a Very Bad Idea (TM)

``` java
// yeah, java would want it wrapped in a try-catch
class Foo implements Cloneable {
  public Foo clone() {
    Foo f = new Foo();
    f.attribute1 = this.attribute1;
    f.attribute2 = this.attribute2.clone();
  
    return f;
  }
}
```

This is a VBI (Very Bad Idea (TM)) because...

![Dave's Runtime Notation Diagram](http://i.imgur.com/GP0dTUG.jpg "Why it's a bad idea")

1. A1: Prone to object slicing

Consider the class `Tiger`, that is a subclass of `Mammal`; class
`Tiger` adds a new attribute `stripes`. Assume the `clone()` method was
defined in `Mammal`. The canonical form would function correctly. But
the VBI version, when asked to clone a `Tiger`, would return a `Mammal`
(with those parts defined in `Mammal`'s superclass(es)). 

> Dave: The Canonical form says: I want one of whatever I am. 
> The VBI says: I want a `Foo`. It's bad, cos it specifies what it
> wants, like a bad boy. 

2. A2: can't access superclass's private attributes

Even if the class has no subclasses, but is itself a subclass of
something with private attributes (w/o accessors), we can neither read
nor wrote those attributes from the scope of this class.

3. A3: a connascence issue

Even if the class has no subclasses, it's still a connascence issue: if
the class's definition ever changes -- it would necessitate modifying
the `clone()` method. 
