---
title: Template Method 
layout: post
categories: notes 
tags: template factory homework
---
# Administrivia
* Read: Template Method(325)

# Factory Method (cont.)

## Implementation (cont.)

## Problem 3 (HW1 discussion cont.)
* ~~3a. Pair a constant method with a FM~~
  * Ditch the FM: just use the CM

``` java
CCreator.getProductClass().newInstance()
```

* `CCreator.getProductClass()` returns a *Class* object

### Discussion: 3 & 3a
> Q: What is the implication of the `productClass` variable not being
> declared **final**? 

Guess: you can possibly change the class

A: its value can be changed; thus the value of `getProductClass()` could
be unpredictable or inconsistent [especially in a multi-threaded environment]. 

Note: we *could* move the responsibility for maintaining the `productClass` variable to the **Creator**. If there is more than one **CCReator** type used in the application, they would be sharing the same variable (hence the same **CProduct**) -- the most recently assigned value would be the one used. [Again,  probably *undesirable behaviour*]

## Problem 4: Parameterise the CCreator with a *Prototype* (117)
* the FM returns clones of the Prototype

``` java
static private Product productPrototype;

static public void setProuct( Product p ) {
	productPrototype = p;
}

public Product makeProduct() {
	return productPrototype.clone();
}
```

> Q: If `productPrototype` is *not* static, what is the implication? 

guess: no guarantee of what the prototype is

A: If `productPrototype` is *not* __static__, then each instance of the
`CCreator` would have its own Prototype object and they could have
different CProducts (can lead to bizarre, confusing systems, similar to discussion on #3)

## Problem 5: Use templates/generics/parameterised classes
Avoids the need to *explicitly* subclass the `Creator` to specify the
`CProduct` type. (Could argue that it's still *implicitly*
subclassing -- it's just the compiler generating the subclass, rather than
the programmer). 

``` java
class Creator<P extend Product> {
	// - do what needs to be done, sirs
	public Product makeProduct() {
		return new P();
	}

	// - 
}
```

### Usage example
``` java
Creator<DaveCoProduct> creator = new Creator<DaveCoProduct>();
```

It could be argued that this is an *alternative* to FM, rather than a
*variant* because it explicitly ducks the issue of subclassing (not to
mention that there is no common type for `Creator<Foo>` and
`Creator<bar>`)

#  Template Method (325)

## Intent
Define the skeleton of an algorithm as an operation, deferring some
steps to subclasses. 

Template method lets subclasses redefine certain steps of an algorithm
without changing the algorithm's structure. 

## Initial comments
* one of the most frequently used design patterns
* it's an example of "good" inheritance
* depends on the *Hollywood Principle* 
	* "Don't call us, we'll call you."
	* the superclass calls an operation defined in the subclass!
		* inverted control structure (the "normal" being when the subclass
		  calls methods defined by the superclass -- i.e., implementation in
terms of services provided by the ancestor class)

## Motivation

### Goal
Define an algorithm *once*, allow subclasses to redefine certain
(preselected) steps as necessary. 

### Physical example

Baking a cake consists of ...

1. Preheat oven
2. Grease pans
3. Prepare batter
4. Pour batter into the pans
5. Put pans in oven and bake
6. Remove pans from oven and cool
7. Assemble the parts
8. Serve and enjoy ;)

Certain steps of this process are *invariant*, while others (3, 5, & 7)
depend on the *kind* of cake being made. The *overall* algorithm is the
same in all cases; we just want to customise how certain steps are
carried out. 

# HW 3 & HW 4

## Homework #2: due Wednesday (11 SEP)
As you may know, the sort() method from Java's Collections class — which
takes an appropriate Comparator object for the type of data in the
collection — performs a merge sort. The Java Design Patterns Workbook
claims that because one step of the merge sort algorithm is being
customized using a Comparator, that it is an example of Template Method.
What do you think? Justify your answer. [200,400] words.

## Homework #3: due Wednesday
Implement a base class Magnitude; instance can be compared to each
other. __Class Magnitude may not have any data members__! It shall, support
the comparison operations:

* `lessThan( m:Magnitude ): bool`
* `lessThanEqualTo( m:Magnitude ): bool`
* `equalTo( m:Magnitude ): bool`
* `greaterThanEqualTo( m:Magnitude ): bool`
* `greaterThan( m:Magnitude ): bool`
* `notEqual( m:Magnitude ): bool`

Note: don't add any other methods to the base class Magnitude; they
aren't necessary and probably will defeat the purpose of the assignment.

### Homework #3: update
Seems I left out an important detail which made the assignment trivial
to circumvent... Class Magnitude may not have any data members! Sorry
about the omission. The description has been updated to include that
restriction.
