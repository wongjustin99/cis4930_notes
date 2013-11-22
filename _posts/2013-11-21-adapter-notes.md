---
layout: post
title: "adapter notes"
description: ""
category: personal
tags: [adapter]
---
{% include JB/setup %}

# Notes on adapter

## Class vs. Object adapters

* A class adapter utilises *multiple inheritance* to adapt one
interface to another. 
  - it adapts by commiting to a __concrete__ `Adapter` class. 
    * it can override some of the Adaptee's behaviour, since the
      `Adapter` is a subclass. An object adapter can't do this, by
just wrapping the calls with other behaviour, since that only allows
for *additional* function calls, versus completely skipping over some. 
  - only introduces one object, doesn't need to have pointers point to
    other things to get to the original adapted object.

The key to a class adapter is that one inheritance branch will be for
the interface, and another inherits the implementation. This is usually
done in C++ by having one private inherited interface, and a public one. 

* Object adapters use object composition
  - The adapter will "have" one of each thing they adapt, and relegate
    functions to the those `Adaptee` objects. 
  - difficult to override `Adaptee` behaviour -- you need to subclass
    the `Adaptee` and point the `Adapter` to the subclass instead of the
`Adaptee`.  
      * which is kinda like a class adapter too, I think

## Questions

### 1. Is a class adapter the same as a 2-way adapter?

Both seem to utilise multiple inheritance. However, 2-way adapters were
singled out as a separate kind. 
