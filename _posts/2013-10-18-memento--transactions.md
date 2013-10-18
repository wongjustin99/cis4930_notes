---
layout: post
title: "Memento & Transactions & Visitor"
description: ""
category: notes
tags: [memento, visitor]
---
{% include JB/setup %}

# Administrivia

* semester more than halfway done
* HW maybe

# Memento (283) & Transactions

A *transaction* is a set of operations, *all* of which must be
completed in order for any of them to take effect. `Memento` will give
us the ability to ...

* roll-back steps prior to the transaction being committed
* undo previously applied operations

## Canonical way of performing a transaction

1. make a `Memento`
2. perform operation(s) on the `Originator`
3. *validate* the `Originator`
4. if theh `Originator`'s state is __inconsistent__, restore the
   `Memento`

## Griffin's "What if" protocol

Alternative to using `Memento` that is based on `Prototype`

1. clone the `Originator` [the `Prototype`]
2. perform operation(s) on the clone -- to be sure they actually work
3. *validate* the clone
4. if statement
  * if *inconsistent*, throw an exception
    * Note: the `Originator` prototype is *unchanged*
  * else 
    * repeat the operations on the `Originator`/`Prototype`

* con: operations must be performed twice (when successful)
* con: some operations "consume resources" and thus, performing them twice
is going to have unintended consequences
* pro: the `Originator` is *never* in an inconsistent state
* pro: object identity is preserved

> Davis: Instead of replaying the operations on the Originator, why not
> replace the Originator with the Clone? 
  
Cos we don't know how many things reference the Originator. 

* pro: object identity is preserved
  - [object identity wouldn't be preserved had we decided (after a
    successful step 3) to simply replace the `Originator` with the
clone]; object identity can be especially important when objects are
persistent in a database. 

# Visitor (331) 

## Intent 

Represent an operation to be performed on the elements of an object
structure. `Visitor` lets you define a new operation without changing the
classes of elements on which it operates. 

## Initial Comments

* the Object structure is usually composed of heterogeneous types
  * each of the different types will execute a *different* method when
    the `Visitor`'s operation is invoked. 
* the pattern is *not* limited to __just__ performing an operation "on
  the elements of an object structure." 
  - it can also be used with __independent__ objects (i.e., ones not grouped
    into a structure) 
  - it can operate over _multiple_ structures
  - it can also be used with structures that are not __typally__ related 

## A bit of Programming History

Assume we have a set of *Shapes* that we want to display...

### A procedural Soluccione

``` java
for each Shape s
  switch  ( s )
    case SQUARE: drawSquare();
    case CIRCLE: drawCircle();
    ...
```

Conditional logic is being used to determine what *kind* of thing to
draw, and -- having identified its type -- to draw it.
 
### Object Oriented Solution

``` java
for each Shape s
  s.draw();
```

Each kind of *Shape* must know how to draw itself. 

* the conditional logic has been eliminated
* knowledge of *how* a particular shape is drawn is encapsulated in that
  *Shape* 

### Visitor-oriented Solution

``` java
Visitor v = new ShapePrintingVisitor();
for each Shape s
  s.accept( v );
```

The `Visitor` is the thing that knows __how__ to draw shapes. *Shapes*
do __not__ know how to draw themselves. 

* conditional logic eliminated
* drawing logic is localised in the `Visitor`

## A "first glance" critique

Visitor centralises the location where *variations* on an operation are
stored. 

* appears to be a major encapsulation violation
* rather than each `Shape` having its own `draw()`, all the variations
  are defined in a single `Visitor` class. 

![Diagram thing]( diagram thing )

> You could overload a visitShape method for each kind of shape, but
> it's not clear then what is supposed to happen.

## Double dispatch

You've been weaned on programming in OO environments that support
__single__ dispatch: 

* That is, the method that gets executed when an `operation` is invoked
  on an object *depends* on that (target) object's *class* (dynamic
binding). 

With __double dispatch__, the **method** that (ultimately) gets executed
depends on the __class__ of *two* objects: in our example here

* the object upon which the __operation__ was invoked, and  
* the object that was supplied as an argument to the request 

![ Second diagram ]( definition of double dispatch )

Note: the act of drawing will almost certainly require the `Visitor`
(specifically the `ShapeDrawingVisitor`) to query the Shape for
geometric data -- e.g., the radius -- but that has been elided from the
SD above. 

In this case, *double dispatch* is implemented by the (concrete) Shape's
`accept()` method, which tells the `ShapeVisitor` which of its
`visitX()` operations it should execute. 

> John: Is this kind of like Template method, with its inverted control
> thong ?  

## Applicability 

* the set of classes which are candidates for being visited must be
  relatively static. 
  - the reason being: every time we add a class to that set of 
visitable classes, the `Visitor` interface must be updated, and hence all the
classes which implement it. 
  - --e.g., adding class `Foo` would necessitate adding a `.visitFoo()`
    
* often, it's not until late in a product's life-cycle that one
  recognises, "Hey! We should have used the `Visitor` pattern!"
  * e.g., creating a complete `Shape` hierarchy only to realise that one
    needs to be able to *draw* them. 
    - thus requiring us to revise each of the classes so that it'll
      support a `draw()` method. 
    - had the `Visitor` pattern been used from the start, all we'd need
      to do is implement an appropriate `Visitor`

* When is `Visitor` *violating* encapsulation? When is it *supporting*
  encapsulation? 
  - a class's interface should support only those operations that are
    essential to that class's core abstraction. 
* tangentially-related operations are __not__ part of the core
  abstraction. 
* thus, we can argue that lumping all the variations (across a
  hierarchy) of a non-core operation into a `Visitor` is actually
supporting *good* encapsulation!

# There's a homework 

OK. 
