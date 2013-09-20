---
layout: post
title: "State (2) & Abstract Factory"
description: ""
category: notes
tags: state [abstract factory] hw redux solutions
---
{% include JB/setup %}

# Administrivia

* Read: Abstract Factory (87) & Builder (97)

# State (305) (cont. 3)

## Implementation issues

### How does the Context get updated on a state transition?

The `CState` will be responsible for "setting" the `Context`'s state
variable directly: 

``` java
context.state = FooState;
```

via an operation: 

``` java
context.setFooState();
```

via a parametrised operation: 

``` java
context.setState( FooState );
```

### How does the CState know its Context?

* the `Context` passes a reference to itself when forwarding a request
to the `CState`: 

``` java
this.state.foo( this );
```

Hey, only the __second__ this up there is required. The first one is
optional. 

* the `Context` is passed to the CState's constructor so that the
`CState` can maintain a reference/pointer to it. 

* [requires language support] `CState` is a *nested class* and
implicitly knows who its context is (and has access to all its private
stuffs).  

### When are CState objects instantiated? (CState stands for concrete
state)

Could be one of the following: 

* as needed
  * discard on transition
  * if the needed `CState` doesn't already exist, or hasn't been
    instantiated
    * instantiate a new one
    * retain the old one
  * else
    * use the retained, previously instantiated `CState`
* when the `Context` is instantiated
  * make a set of al possible `CState` objects

### Where should data that is specific to one particular state be
localised? 

Any data that is specific to *a* state should be localised in the
corresponding `CState`. 

# HW4 redux 

> Q: If you really could change class, would there bea ny reason to use
> the `State` pattern? 

A: an (potential) unintended consequence: we could no longer use
subclassing to extend `Context`-related behaviours; that's because
instead of extending a *single* `Context` class, we would need to extend
*all* the `StateContext` classes. So, yes, there.  

# Abstract Factory (87)

## Intent

Provide an interface for creating families for related or dependent
objects without specfiying their concrete classes.

## Motivation

The notion of a `MenuBar` having `Menu`s which are in turn composed of
`MenuItem`s (and `Menu`s are themselves `MenuItem`s!) is a pretty common
theme in GUI toolkits. 

__AbstractFactory__ lets us create instances of different product
classes that are meant to work together. [In java, you can't mix AWT,
SWT, & Swing components. In C++, you can't mix QT, wxWidget, and GTK
menus &c.] 

## Structure

!["Abstract Factory
structure"](http://www.silversoft.net/docs/dp/hires/Pictures/abfac108.gif
"UML diagram")

* ConcreteFactory1 will make ConcreteProdA\* kind of stuff. 
* ConcreteFactory2 will make ConcreteProdB\* kind of stuff. 

## Implementation

### Type 1: Abstract Factory with FactoryMethods (AF with FMs)

1. Use inheritance to override the FMs
  * the `Creator`, that is, the `ConcreteAbstractFactory` (CAF) is
    abstract (or an interface)
  * subclasses must implement the FMs -- one for each `Product` type
    * could also use Constant Methods (per our earlier FM discussion)
2. Use inheritance to override the initialiser
  a. FMs are written in terms of *Class* objects
    * the `Creator` is abstract, and has instance variables for each
      `Product` type. 
      - those instance variables __hold__ the class
      objects
    * the FMs are concrete: the Class objects are used to intantiate
      products.
    * override the initialization code for each CCreator so that it will
      instantiate the appropriate Class objects for each Product type.  

    ``` java
    this.fooClass = Class.forName( "DaveCoFoo" );
    this.barClass = Class.forName( "DaveCoBar" );
    ```


