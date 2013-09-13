---
layout: post
title: "Template Method (3)"
description: "Finish up Template Method, HW3 solution, and intro to Strategy"
category: notes
tags: homework solutions template strategy
---
{% include JB/setup %}

# Administrivia

* None

# Template Method (325) continued

## Implementation (cont.)

### Modifiers & methods

> Q: Which access modifiers (in languages which support them) should be
> used with the template method and the operations it calls? 

* Protected, for required ops

| modifier            | method                   |
| ------------------- | ------------------------ |
| public/protect | concrete (when generally useful, not TM specific)
| protected | hook |
| abstract protected  | primitive |
| private | invariant steps (concrete methods, not template-specific )
| note1 | factory methods | 
| final + note2 | TM |

* note1: depends on whether: 
  * there is a default implementation (whether __abstract__ or not) 
  * who else should be able to use it (whether __public__ or __protected__) 

* note2: depends upon who is supposed to call it
  * therefore, __public__ or __protected__ or __private__
    
## Some things to ponder on your own (or with classmates)

Davis will remain unusually silent on these topics. 

* Exam questions? 

> Q1: Is an *inverted control structure* an integral part of TM? Justify
> your answer. (Could you do Template Method without an inverted control
> structure?)

> Q2: TM relies on *inheritance*. Would it be possible to get the same
> functionality using *object composition*?  If so, what would be some
> of the trade-offs? If not, why not? 

John: No, since methods are being composed, not objects. 

> Q3: Isn't TM just plain ol' *procedural decomposition* gussied up for
> OOP? Justify your answer. 

## HW3 (Magnitude) redux

> Q: Could class `Magnitude` have a datamember, and therefore a `getMagnitude()`
> operation? Would you have the same functionality?  

A: Not only can a `getMagnitude()` *not* be defined in the base class
`Magnitude`, it's impossible to declare it, because there's no common type
that *all possible* instances of `Magnitude` can be reduced to -- i.e.,
we don't know what *type* subclasses will use to represent a magnitude. 

> Q: What was meant by " __Magnitude__ is not meant to define a type?"

A: Instances of subclasses of `Magnitude` are *not* substitutable for
`Magnitude`. It's just a some implementation required
 by subclasses, and not any kind of comparisons can be made to each other. 

## Strategy (315)

### Intent

Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from the clients that use it. 

### Example applications

* video compression
* encryption
* bitmap representation (.jpg, .png, .gif, &c.)
* data visualisation (tabular, char: bar, line, pie, ...)
* GUI component layout managers
* auto insurance quotes
	* states have different regulations & assess penalty points in different ways

### It's an alternative to *brute force* 

``` java
switch ( whichAlgorithmToUse ) {
	case alg1: alg1( data );
		break;
	case alg2: alg2( data );
		break;
}
```

### Facilitates Code Reuse

When algorithms are embedded in the client's code, it makes it hard/impossible to re-use the algorithm's implementation in different context. 

> Q1: Why not just lump all the algorithms together in a __utility class__ that could be shared by multiple applications? [`FooAlgorithm.daveFoo()`]

* A1: (assuming we want to choose the algorithm at runtime,) we still need to make a choice, thus necessitating conditional logic. 

* A2: *all* the algorithms would need to be memory resident, even though the application might only use one of them. 

* A3: clients would need to be modified when new algorithms were added to the utility class (in order to use the new algorithm). 

* A4: adding new algorithms would entail modifying the utility class: what happens when we don't have access to the source code? 
