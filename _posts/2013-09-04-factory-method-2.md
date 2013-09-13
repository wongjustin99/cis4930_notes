---
layout: post
title: More on Factory Method (2) 
categories: notes
tags: solutions factory template
---
{% include JB/setup %}

# Administrivia
* Read: Template Method (325)

# HW01: Discussion

## Problem 1
A great answer will capture: 

* the client (of the FM) wants an instance of a *type*---it does *not*
  specify which implementation it wants
* the client of BorderFactory wants (and requests) a *specific*
  implementation of type Border. 

Exam questions may be in this format. Requires understanding, and not
regurgitation. A following question might be: 

  > Q: Since the programmer ultimately chooses the ConcreteCreator, (and
  > thus implicitly the ConcreteProduct), is the use of a FM "morally
  > equivalent" to the programmer choosing a Border class with a call
  > to the BorderFactory?

Two different answers depending on an initial assumption you make. 

### A1: assume BF versus PFM
* BF could have been implemented to take a parameter to specify the
  Border type
* PFM already takes a parameter

### A2: assume BF versus FM
* BF can return a shared object
* FM intent is that it returns a new object

No, clients of the FM say, "Give me the right kind of Product", without
specifying what it is. BF clients say, "I want this specific kind of
Product."

![Rabbit and Tiger Graph](insert from phone)

## Problem 2

> Q: In your own words [200,400], *how does FM promote loose coupling*?

Programming to an interface, not an implementation. 

## Problem 3

No: only *one class* of Object is ever created.  [same reason
**toString()** isn't a FM!]. It doesn't matter that there is a parameter
problem, just that there's only one type of Product. 

# Factory Method (107) [cont.]

## Implementation (cont.)
1. on previous day
2. on previous day
3. Pair a constant method with a FM
	* Note: a *constant* method is one that always returns the same thing. 

> Note: the technique discussed here requires language support for representing classes as objects. In Java, that mechanism exists as `java.lang.Class`

* The **Creator** implements the FM: 

``` java
public final Product makeProduct() {
	return getProductClass.newInstance();
}
```

* The **CCreator** implements the *Constant Method* `getProductClass()`: 

``` java
static private Class<? extends Product> productClass = _____;

public final Product makeProduct() {
	return CCreator.productClass;
}
```

*or*

``` java
public Class getProductClass() {
	return CProduct.class;
}
```

* In that blank: a *Class* object for the desired *CProduct*

### Java stuffs...
* 3 different ways to get a *Class* object representing *FooClass*
  1. `FooClass.class`
  2. `fooClassInstance.getClass()`

