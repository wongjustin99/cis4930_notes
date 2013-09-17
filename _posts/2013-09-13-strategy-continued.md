---
layout: post
title: "Strategy Continued and State"
description: "More on Strategy, HW02 redux/solutions, State pattern"
category: notes


 
  
  
tags: strategy homework solutions state
---
{% include JB/setup %}

# Administrivia 

* Read: State (306)

# Strategy (315)

## Structure
!["UML diagram of
participants"](http://www.silversoft.net/docs/dp/hires/Pictures/strat011.gif "Structure of Strategy")

## Usage

* the `Context` (the Strategy's client) works with a `CStrategy`
  * _may_ use the same Strategy for the program's entire execution
  * _may_ use different Strategies as the program execute

* the `CStrategy` may be set: 
  - a _mutator_ on the `Context` which allows one to choose the
    `CStrategy`. 
  - passed as a parameter to the `contextInterface()`
  - specified as a *type parameter* (when `Context` is a
    generic/template/parametric class)

* Any time the client needs the results of the algorithm, it forwards
  the request to the `CStrategy`. 

## Implementation

Must choose between a "push" model, or a "pull" model. 

### Push

The client is responsible for passing along the needed data with the
request. 
  
  * con: must pass *all* the information that *any* member of the
    algorithm family might need. 
    - This is because we have to provide a single interface
      (`algorithmInterface()`) -- that is usable with *any* `CStrategy`.
(some may require information that others do not). 
    - Further, since we shouldn't make any assumptions about *which*
      `CStrategy` will handle the request, we can't safely pass *null*
      values for things we think the algorithm will want. 
      * This matters more for passing by-value, than by-reference. 

### Pull

* The `CStrategy` object requests the data it needs from the client. 
  * how does the Strategy know who the client is (so it can make the
    request)?

####  Pass a reference  when *constructor* is invoked

* One way of doing this is to: pass a reference to the client when
  the `CStrategy`'s constructor is invoked. 
  * con: You can't share the Strategy object between multiple
    clients cos it will only pull data from one specific client. 
  * pro: makes for clean code 

#### Pass a reference when *algorithm* is invoked

* Pass a reference to the client when the *algorithm* is invoked on the
  `CStrategy`. 
  * pro: It's a sharable object
  * pro: makes for clean code

Pull Strategy Overall Con: tighter coupling between client and the `Strategy`

* The Client *must* support a known interface in order for a `Strategy`
  to query the client for the needed data. 

# HW02 (java's `sort()` takes a comparator) redux

## Template Method? Or Strategy? 

* How is that `Comparator` being used? 

### TM? 

* It *is* defining how objects can be compared to each other
* However, at a higher level of abstraction, it: 
  * is being used to *customise* a _step_ in a (sort) algorithm

This sounds like a Template Method. However, TM uses *subclassing* to
redefine a step's behaviour. 

### Strategy? 

* passing an object that implements an algorithm  (the `Comparator`)
  that is suitable for comparing the actual type of data in the
  collection. 
* However, Strategy relies on *object composition*
  * So, NO

### Functor

A *functor* in OOP, is an object that represents a function. They're
useful, even in a language that has function pointers -- like C++. 

  * In Java, you need to encapsulate functions into objects.
  * In C++, you could just pass a function pointer. 
    * they are useful because they *are* objects, and have state (and
      may have additional operations). 



 
  
  
There are a lot of people who would argue that the use of `Comparator`
by `sort()` is an example of the `Strategy` pattern. However, I would
argue that it is really an example of the use of a functor, because it
is being used to parameterise an *operation* rather than an *object*. 

In the spirit of full disclosure, there are some that believe that
Functor...

  > "...is a perfect realisation of the Strategy pattern..."

They may be right, and Sr. Davis may be splitting hairs. But if he is
splitting hairs, why do the GoF mention Functors in their discussion of
the `Command` pattern, but completely ignore them when discussing
`Strategy`? 

* Davis thinks they did not overlook this, because the consequences of
  using a Functor are different than using a Strategy. The whole point
  of using Strategy is using subclassing. 

# State (305)

## Intent

Allow an object to alter its behaviour when its internal state changes.
The object will appear to change its class. 

## Structure

![](http://www.silversoft.net/docs/dp/hires/Pictures/state-eg.gif
"Structure of State")

## Applicability

Two main cases where it is usually applicable: 

* Use it when an object's behaviour is *state-dependent* -- where the
  object's state is determined at runtime, and may change. 
* Use when state-dependent behaviours may be implemented by *multiple*
  operations. 
  * i.e., when an object changes state, the behaviour of multiple
    operations may be affected. 


