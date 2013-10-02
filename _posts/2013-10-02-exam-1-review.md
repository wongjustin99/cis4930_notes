---
layout: post
title: "Exam 1 Review"
description: ""
category: notes
tags: [exam, redux]
---
{% include JB/setup %}

# Administrivia

* None

# Exam 1 redux

## Round 1

When inspecting source code, there is a simple, sure-fire way to
distinguish between an `Abstract Factory` (in which the prouct type is
selected by passing a token) and a `PFM` that does *not* involve looking
at the definition's body! What is it? 

### AF returns a concrete factory where.. 

NOPE

### One's a class and one's a method

*Almost* - One's an object, and one's a method. 

## Round 2

One way to view design patterns is that they encapsulate some aspect of
a system that can vary. For each of the patterns we've covered (FM, TM,
Strategy, State, Builder, Prototype, Decorator, & AF) what is the aspect
that varies, and how does the pattern support this?

### FM 

####  Varies the object returned 

Insufficient answer

#### The clas of the object varies, but they all have the same time

### Template Method

#### 

### Strategy

#### Algorithms vary

Works by using object composition and message-forwarding. The
restriction is a common interface. 

### State

#### Class's behaviour

Accomplishes it by encapsulates the behaviour of the state to separate
object *AND* uses message forwarding. 

### Builder

#### The things constructed are changed. 

Representation is separate from process.

Accomplished through The director is not strictly necessary. In the
past, he's asked, "Since the director is the thing controlling the
process, would it be better to name it `Builder`?" No, Cos the Director
is not the essential part of the pattern, the Builder is. 

### Prototype

#### Allows you to vary the Kind of object being created

Accomplished through letting the `Prototype` object to be cloned and
modified. 

### Decorator

#### Allows you to extend/replace functionality of an object

Accomplished through object composition of the same exact type, so you
have object transparency. (Saying composition alone isn't enough). 

Wrapping it with an object that has the *exact* same interface. And
you're forwarding the requests to the wrapped object. 

### Abstract Factory

#### Family of project instantiated is varied

Accomplished through inheritance, assuming the AF is an interface, you'd
have classes that implement that interface, that then instantiate things
from there.

## Round 3

You have been asked to peruse code that was neither commented nor
followed the GoF's advice to use names which are both context
appropriate and allude to the pattern being used. How would you
distinguish between the use of the Strategy and State patterns?

### States manage transitions

Strategies do not. 

## Round 4

Consider the following list: name, problem, solution, & consequences.
Why is that list *insufficient* to define a design pattern? *Justify
your answer*

### Context

Why it was appropriate so that the DPs can't be indiscriminately
applied. 

## Round 5

Argue convincingly for or against the proposition, "Boilerplates need to
be customised. The `Builder` pattern would be a reasonable choice for
implementing the customisation mechanism."

### There is an acceptable preffered answer, and an acceptable, but only
under certain conditions, answer. 

#### It *is* an appropriate choice

#### Best answer: no

Builder is applicable when you have different representations. If you're
doing different variations on the same representation, then Builder is a
poor choice. 

#### Yes (hinges on the above)

Only correct if you say that the customisations have different
representations. 

## Round 6 

Consider a method `foo()` in some base class; in general, if `foo()` calls
a `FM`, is that sufficient to make `foo()` an example of a `TM`? 

### Not sufficient

If every method that called a FM was a TM, then  it's silly. 
