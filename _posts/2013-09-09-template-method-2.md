---
layout: post
title: "Template Method (2)"
description: ""
category: notes
tags: template homework
---
{% include JB/setup %}

# Administrivia

* Read: Strategy (315)
* HW 2 & 3 online (due Wednesday 9/11)

# Template Method (325) continued

## Overview of Implementing TM

There are several kinds of operations that can be used to implement a TM

* *Concrete* ops (either on the **Concrete** class or client classes)
* *concrete Abstract class* ops
* *primitive* ops
* *factory methods*
* *book* ops 

If this "concrete op" on the Concrete class isn't a concrete abstract
class op, primitive, FM, or hook, then what the heck is it? How can I
(as the Abstract class) possibly foresee operations that the
**Concrete** class *adds* to its interface????

This list really boils down to three...

| Method               | Abstract class's role     | Concrete class's role |
| -------------------- | ------------------------- | --------------------- |
| Concrete             | define                    | *not* override        | 
| Primitive            | declare                   | *must* override       |
| Hook                 | define default behaviour  | *may* override        |


Often hooks do *nothing* by default; hence it is said to provide a
"hook" for extending the TM's algorithm (at controlled points)

> Q: Why didn't FM make my condense list of op types used by TM?

A: Because it's just a special case of ...

  * in the case when there *is* a reasonable default behaviour, then
    it's a *hook*
  * otherwise, it's a *primitive*

That said, it's still useful to say, "FMs can be used in a TM", because
of the semantic meaning: FM's purpose is to create instances of a type. 

## Implementation Details

1. *access control modifiers* can be used to ensure that *primitives*
   don't "leak" into the type's interface. Declare primitives as
__protected__ instead of __public__.

2. *minimise the number of primitive ops* -- the more primitives one
   declares, the more work clients have to do. Therefore, if there is a
reasonable default behaviour, make it a *hook* instead. 

  * Note: there is a potential downside to this advice: if one forgets
    to implement a primitive, the compiler will yell at you. But if you
forget to override a hook, the compiler won't give any warning.

## Designing and refactoring hierarchies for reuse

General rule of thumb: move *behaviour* __up__ in the hierarchy, move
*state* __down__ in the hierarchy. 

### Ken Auer's 4 heuristic 

A heuristic doesn't guarantee correctness, while an algorithm does

1. define classes by *behaviour*, not *state*
  * focus on "what does it do?", not "what does it know?"

2. implement behaviour with abstract state
  * behaviours should be written in terms of *accessors* and *mutators*

3. implement behaviours in terms of a small set of override-able "kernel
   methods"

4. defer identification of state variables
  * accessors and mutators are *declared* in the __base__ class, but
    *implemented* by the subclasses. (specifically, those in which the
data members are defined) 

Applying these 4 heuristics will give you a Template Method. 

## A rather primitive method (har har) for generating TMs

1. implement everything in a single, well-commented method
2. based on the comments, divide into logical steps
3. implement those steps as methods
4. replace steps with calls to those methods
5. use *constant methods*
6. lather-rinse-repeat for each step

## Good programming style guideline

Every statement in a method should be at the *same* abstraction level

Compare: 

``` java
balance += deposit; 
```

and 

``` java
updateBalance( deposit );
```

Both accomplish the same thing, but the latter is at a higher
abstraction level. 

## Applying TM in your very own programs

Some advice... 

* eschew constants: use *constant methods*
* eschew constructors: use *factory methods* 
* isolate variable parts of an algorithm into overridable methods 



