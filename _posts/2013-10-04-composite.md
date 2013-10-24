---
layout: post
title: "Composite"
description: ""
category: notes
tags: [homework, decorator, composite]
---
{% include JB/setup %}

# Administrivia

* Read iterator (257)
* Read Composite (16#)

# Decorator (175)

## Compared to other patterns

##3 Adapter

Sometimes `Decorator` is confused with `Adapter` -- both are known as *wrapper*

* Decorator
  * Has the *same* interface as the object being wrapped.
  * purpose: extend/replace the wrapped object's behaviour
  * can be nested/chained (because they do not have the same interface)

* Adapter
  * *changes* the wrapped object's interface expected by the client and the object's actual interface.
  * nesting adapters (with the *same* interface) is nonsenseical.

* Proxy

 Proxy, like decorator, requires a *conforming* interface. But..

* Proxy's purpose is to control access to the wrapped object
  * It may enable/disable behaviours
  * Does not extend/replace behaviours
*Proxies are, in general, not nestable
* A `Proxy`'s implementation may be the same as `Decorator` (but may also be quite different).

> Davis; It may look like a duck, and QUACK like a duck, but it's NOT!

* Composite

Decorators and Composites are often used together

* In such cases, they are usually derived from a common base-class/interface. 
* trees may be decorated, and a tree may contain decorated subtrees

  * Decorator
    * has *exactly* one child
    * purpose: extend/replace behaviours

  * Composite
    * may have *zero or more* children.
    * **aggregate** objects and treat them as one

# Composite (16#)

## Intent

Compose objects into tree structures to represent whole-part hierarchies.

Compositive lets clients treat individual objects and compositions of objects uniformly. 

## Initial Comments

__TRANSPARENCY__ is the key to this pattern ( along with many others ). 

Transparency: clients treat individual objects, and object-structures the same way.

* Consider three ways a tree may be structured

![Diagram of # tree structure-types]( INSERT_URL_HERE )

Top-down, Bottom-up, and doubly-linked

##3 When would each tree type be sued with _Composite_ ?

* Top-down:
  * When the parents do not cache state *or* 
  * when the children's state does not change (in a way that affects the result of Component operations)

* Bottom-up
  * Never! Cos we'd be interacting with the bottom guy, who knows *nothing* about his children.

* Doubly-linked
  * When the `Composite` (parent) caches values based on *previous* interactions with its children; if the children can change state, then we need a mechanism for notifying their parent [either to invalidate the cache, __or__ cause the parent to take some appropriate action, such as recomputing the cached values. 
  * 
> Davis: If a child can change state and we're interacting with something upstairs (and it's interacting with a cached value), we need some mecahnism where children can cause the cached value is updated, or at least notify the parent that the cached value has been invalidated. 

## Structure

![UML Diagram]()

## Example: Structured graphics representation

Consider the following lovely logo:

![Photo of a dream cloud]()

A likely object structure for representing this might be: 

![Tree strucutre photo]()

Pictures can *contain* pictures __AND__ primitives. 

> Davis: What are some likely operations that you'd want to invoke on such a structure?

> StudentBoy: DrawYourself()

Some likely operations one might want to invoke on such a structure include:

``` java
drawOn( surface );
getBoundingBox();
```

Example: product packaging

Shipping container => Crates => Boxes => Packages => Bags => Product

Shipping containers may contain Creates, May Contain, Blah...

Some likely operations: 

``` java
getWeight()
getVolume()
getValue()
makeShippingManifest() // what *is* in this shipping container?
```

> Davis: Unless a product is perishable, or its price is voltaile, the return values (for a given object structure) will likely be computed just once, and subsequent invokations would use a cached value. 

> Davis: That will *probably* be the first time the operation is invoked. 
