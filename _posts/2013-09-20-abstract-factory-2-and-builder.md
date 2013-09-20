---
layout: post
title: "Abstract Factory (2) and Builder"
description: ""
category: notes
tags: [abstract factory, builder, homework]
---
{% include JB/setup %}

# Administrivia

* Read: Builder (97)
* Read: Creational Pattern chapter introduction *and* the concluding
  discussion

# Abstract Factory (87)

## Implementation (Cont.): Implemented AF as a Factory Method

### 1. Use inheritance to override the FMs

### 2. Use inheritance to override the initialiser
  a. FMs are written in terms of Class objects
  b. FMs written in terms of Prototype objects

* `Creator` is abstract and has instance variables for each `Product`
  type
  * hold `Prototype` objects
* FMs are concrete: the `Prototype` objects are used to instantiate the
  `Products`
* override the initialisation code for each `CCreator` so that it
  instantiates the appropriate `Prototype` objects for each `Product`
type

``` java
this.fooClassPrototype = new DaveCoFoo();
this.fooClassPrototype = new DaveCoBar();
```

### 3. Parameterise the AF

a. FMs are written in terms of Class objects
  * similar to #2, except the AF's initialiser is parameterised with
    objects that will be used to instantiate the Products. 
b. FMs are written in terms of Prototype objects

### 4. Parameterise the AF with a Product family name

(this one is a little more sophisticated)

* *requires* that the `Product` class names *strictly adhere* to a
  regular naming convention. 
* The `Creator` is concrete and has instance variables for each
  `Product` type
  * holds `Class` objects
* FMs are concrete: the `Class` objects are used to instantiate the
  `Products`.
* the `Creator` is initialised with a *product family* name. 

``` java
this.fooClass = Class.forName( familyName + "Foo" );
this.barClass = Class.forName( familyName + "Bar" );
...
```

### 5. AF is a parametric/generic/template class

* the parameter names used to instantiate the class will specify the
  `Product` types.

``` java
class AF( ProductA, ProductB, ..., ProductZ )
{ .. }
```

## Implementation: AF implemented as a Simple Factory

See the discussion we had on *Simple Factories*

Con: AFs often return wildly different `Product` types (that may or may
not share a common ancestor; since the SF is a single operation, with a
strongly-typed language, we'll be forced to declare a generic return
type. (Probably Object, or void\*) ). This will force the client to use
 casting to coerce the return value to the expected type. 

Pro: Easier to extend the AF to support additional `Product` types than
a FM-based approach. The FM-based approach would require adding a new
method to each AF class. ( ANSWER TO THE 2nd QUESTION , COS HE DID IT ON
ACCIDENT )

# HW05

1. Implmentation techniques 3 & 5 require adding *something* to configure
   the AF. Should that knowledge be localised? Where? [200,400]
2. ~~Why is it easier to extend a SF-based AF than a FM-based one to
   support addtional operations?~~ (Struck, cos we don't need to do it,
MAN!)

# Builder (97)

## Intent

Separate the construction of a complex object from its representation so
that the same construction process can create different representations. 

## Motivation

Consider the following process of making a 
bacon+pineapple+jalapeño pizza: 

1. Prepare some dough
2. Slabber it with some sauce
3. Sprinkle with cheese
4. Sprinkle the toppings
  * bacon
  * pineapple
  * deliciously spicy jalapeños
  * ching-chongs

!["Three girls surrounding a Pizza!"](http://i1.ytimg.com/vi/G1cQkR8oiHY/maxresdefault.jpg
"Gimme Pizza!")

### There are many possible variations

1. Within the same product type 
  * small, medium, large, XXXXXXXXXXXXXXXXXXXXXXXXXXXLarge

2. Within the *actual* product type
  * the *kind* of the product type you get is different
    * this same process can be used to *draw* a pizza. everyone imagined
      a pizza you can "eat" (if you know what I mean). 
  * depictions of pizzas (i.e., images)
  * recipe: step-by-step instructions for making the pizza

### What in the heck is a *representation*? 

The actual class of object(s) that are configured and returned by the
construction process. 

## Structure

!["UML diagram of Builder
structure"](http://silversoft.net/docs/dp/hires/Pictures/builder.gif
"UML diagram")

Builder knows how to build a *particular* representation. It knows
nothing about process. It only knows about representation. 

The Director knows the process. 

