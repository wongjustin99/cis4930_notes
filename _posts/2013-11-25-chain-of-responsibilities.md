---
layout: post
title: "Chain of Responsibilities"
description: ""
category: 
tags: [project, chain of responsibilities, redeaux]
---
{% include JB/setup %}

# Administrivia 

* Mon 12/2 - no klass
  * presentations [term project deliverables due]
* TUESDAY 12/3
  - Presentations [postmortems due]
* Wednesday 12/4
  - class wrap up

# Chain of Responsibilities (223)

## Intent

Avoid coupling the sender of a request to its receiver by giving more
than one object a chance to handle the request. Chain the receiving
objects and pass the request along the chain until an object handles it.

#A# Initial comments

There is no guarantee that *anything* will handle the request!

## Runtime structure

While it may be take a linear form, it more frequently a tree of
responsibilities. 

![generality diagram]()

> Davis: If you're clicking on a button, the first thing that would try
> to reach it is more specific, then things outside of the group. 

## In pseudo code

```
if I can handle the request
  handle the request
else if I know of a more general handler
  forward the request
else
  ignore the request
```

There will only be *one* other handler, in the canonical pattern. This
is because it makes it simpler. 


# Exam 2

# Exam 3

## Problem 1

Bridge vs. Pluggable Object Adapter

no reflecxion

## Probelm 2

See chapter 2 in OOSC, 2e

## Problem 3

What makes a Tailored Object Adapter different from ordinary object
composition?

OC is a mechanism that enables a class to be written in terms of
services provided by other classes. TOA is a special case of OC --
difference lies in the *intent* of TOA: it is specifcially making an
existinc lass's interface compatible with the interface that client's
expect, thus enabling instances of the existing class to provide
services. 

## Problem 4

Implement `MutableString`

Should have used a const for the String in SharedStringRep. Also should
have returned an *reference* to a String instead of a String itself. 

Added the count decrementing and `delete` in one function with an if
statement. 

## Problem 5

definitions out of the book

## Problem 6

They both deal with communication between objects, and are competing in
that space. 
