---
layout: post
title: "State & Abstract Factory"
description: ""
category: notes hw
tags: [abstract factory state hw]
---
{% include JB/setup %}

# Administrivia

* Read: Abstract Factory (87)

# State (305) continued

## Intent

Allow an object to alter its behaviour when its internal state changes.
The object will appear to change its class. 

## Applicability (cont'd)

* use when state-dependent behaviours may be implemented by *multiple*
  operations
  * i.e., when an object changes state, the behaviour of multiple
    operations may be affected.

``` java
class Order {
  OrderProcessor processor = ...;
  StateToken state = CREATED;

  submit() {
    switch ( state ) {
      CREATED : 
        error( "Must be validated first..." );
      VALIDATED: 
        processor.submit( this );
        state = SUBMITTED; 
      SUBMITTED: 
        error( "Can't submit same order twice, yo" );
      CANCELLED: 
        error( "This order has been cancelled, yo" );
    }
  }
  
  validate() {
    // also has state-dependent behaviour
    switch ( state )
    {
      // ...
    }
  }
}
```

Talking about when one or more operations depend on the current state.
We are trying to do this with switch statements, or a big block of "if"
statements. 

Note: there are multiple methods that have conditional logic used to
determine *what* to do depending on the `Order`'s current state.  

In terms of this specific implementation: 

  * must execute decoding logic __every__ time `submit()` is called. 
  * similar control structures exist in other methods. 
  * state transitions are *embedded* in those methods. 

From a code understanding & maintenance perspective: 
  
  * It's relatively hard to grasp the relationship between states
    *because* the transitions are buried in the code. 
  * Adding new states can be laborious and error-prone because *every*
    method containing the decoding structures will need to be updated. 
    * the compiler won't be able to remind us about methods that you may
      have forgotten. 

* use when the context has a limited, well-defined set of possible
  states
* context must have defined transitions between states. 
  - it's inappropriate to use the __State__ pattern when state changes
    are inconsistent, or subjective. 

## Structure

!["UML diagram of
state"](http://silversoft.net/docs/dp/hires/Pictures/state.gif "State
Structure")

## Example: translated to State pattern

The State pattern encapsulated state-dependent behaviour in 
*__separate__ state objects* 

``` java
class Order {
  OrderProcessor processor = ...;
  StateToken state = new CreatedState();

  submit() {
    state.submit();
  }

  // ...
  
}

class CreatedState extends State { // or implements, if you prefer
  
  submit() {
    error( "Must be validated first." );
  }

  // other state-dependent operations defined as well

}

class ValidatedState extends State { 
  
  submit() {
    processor.submit( thisOrder );
    thisOrder.state = new SubmittedState();
  }

  // more defined methods, BLAH BLAH

}
```

und so weiter...

Note: Don't get hung up on how State objects communicate with the
Context. 

There are several ways this can be implemented. 

  * some languages (e.g., Java) have *nested classes* 
  * can also pass a reference to the context when constructing the state
    object.
    * pass it to the state constructor
  * pass a reference to the `State` object when invoking its methods

### Check it out, yo: 

The control structure has been eliminated. 

* state transitions are *more* explicit. 
  - no longer buried in the middle of a conditional block
* overall easier to understand and maintain

## Class Changer Definition

> "The object will appear to change its class"

What does this mean? 

A `class` defines the properties and behaviours of its instances;
because the object's behaviour may vary dramatically depending upon its
state, that led the GoF to use the phrase "will appear to change its
class." They are focused on a conceptual definition of class, as opposed
to a code definition of class. From the perspective of the defining
code (and type system), the class doesn't change. 

The distinction here is between:

* apparent class -- what kind of thing does this object *behave* like? 
* actual class -- which class' constructor was used to instantiate this
  object?  

### What if you had a language that allowed the *actual class* to change? 

It would allow the use of combined State-Content objects. (remember, in
the canonical pattern, we're using different objects to change the
Context's behaviour.)

Thus, there would be different State-Context classes, one for each
state, and the object would migrate between them as its state changed. 

### What kind of mechanisms could be used to implement this? 

1. `become( instanceOfAnotherClass )`
  * first, we'd need to instantiate `instanceOfAnotherClass` and
    initialise it with *this* object's current attributes. 
  * `instanceOfAnotherClass` may have fewer, or *more* attributes than
the class from which we are transforming. Thus, we'll stipulate that
the set of classes which one can migrate must support the union of
the attributes *required* by each of the classes in the set. 
      * that's not to say it won't have non-essential ones.
  * finally, point all references to the original object to
`instanceOfAnotherClass`
  * the language SmallTalk supports this!

2. `changeClassToThatOf( AnotherClass )`
  * we'll require that `ThisClass` and `AnotherClass` be derived from a
    common __base class__. 
    - note: this was *not* a requirement of `becomes()`
  * the subclasses may *not* define additional instance variables
  * supported by IBM's VisualWorks Smalltalk

this idea of type migration is not theory, it's been put into practice.

### Consequences of migrating classes

It makes subclassing the `Context` more difficult. 

# HW04

If you really could change class, would there be any reason to use the
`State` pattern? [200-500]





