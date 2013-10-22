---
layout: post
title: "Visitor (2) & Proxy"
description: ""
category: notes
tags: [visitor, iterator, proxy]
---
{% include JB/setup %}

# Administrivia 

* None (I think? I didn't go to classen)

# Visitor

## Who controls the traversal?

###  Something *external* to the `Visitor`

That thing would be responsible for calling `accept()` on
    successive elements of the structure being visited. 

* Pro: can support multiple (simultaneous) traversals
* Pro: can have finer-grained control over traversals

### The Visitor itself

Traversal code isn't easily reusable when encapsulated in the `Visitor`

But... What if it uses Iterators? 

## What does a `Visitor` *do* during a traversal?

* There are many possibilities; among them...
  - Modify (some of) the visited objects
  - Accumulate state based upon the attributes of the visited objects
  - Construct an object structure based on the visited objects

## How is the pattern normally implemented? 

### Define the `visitable` classes

Recall: while it's often the case that __is-a__ type relationship among
the visitable classes, it's not a requirement that they have a common
type.

``` java
Class Foo {
  public void accept( Visitor v ) {
    v.visitorFoo( this );
    // ...
  }
}

Class Bar {
  public void accept( Visitor v ) {
    v.visitorBar( this );
    // ...
  }
}
```

### Define the `Visitor` interface

``` java
Interface Visitor {
  public void visitFoo( Foo f );
  public void visitBar( Bar b );
}
```

### Define the `Visitor` class(es)

``` java
Class TypePrintingVisitor implements Visitor {

  public void visitFoo( Foo f ) {
    System.out.println( "Woo, hoo!, I'm a foo!" );
    // note: the visitor is __not__ obligated to "use" the visited
object
  }

  public void visitBar( Bar b ){
    System.out.println( "I'm visiting a Bar, so don't let me drive the
car" );
  }

}
```

## Sheer Evil

A naughty trick for those using languages with reflection/introspection
abilities... 

Assume there is a single superclass of all things that can be visited. 

With proper black arts, one can write a *single* `accept()` in that
superclass -- which would be inherited by all of its children. Hence,
they don't need to implement their own `accept()`. 

### The idea in pseudo-code

``` java 
  Class SuperType {
    public void accept( Visitor v ){
      v[ "visit" + getClass.toString() ](this);
    }
  }
```

### The idea in close-to-executable Java

``` java
Class myClass = this.getClass();
Class visitorClass = v.getClass();

Method m = visitorClass.getMethod( ("visit" + myClass.toString() ), new
Class[] {myClass} );

m.invokeon( v, new Object[] {this} );
```

### On your own ...

Why is this evil? 

> John: Since it assumes that every visitable thing has an appropriately
> named `visitor`. I guess, if everything sticks to that naming
> convention, it's okay. 

# Proxy (207)

## Intent

Provide a surrogate or a placeholder for another object to control
access to it. 

## Structure

Looks a heck of a lot like `Decorator`

![ Picture ] ()

## Four Major Variants

### Remote Proxy

* Stands in for an object that lives in a different *address space* 
  - e.g., another JVM
  - e.g., another node on a distributed (networked) system
  - e.g., etc.
* Hides the fact that the subject doesn't exist in the local address
  space.
  - encapsulates the protocols necessary for communication between the
    address spaces
    * hence, that communication is *transparent* to the client. 

### Virtual Proxy

Allows one to delay the creation of "expensive" objects until such time
as they are *really* needed (if ever).

![Page Layout example] (http://i.imgur.com/FZ7aWHy.png "A page layout of text")

For example, using a proxy for an: image in a page layout/desktop
publishing application.

This allows the application loading the (decompressed) imageinto memory
until it is really needed. 

Using a proxy enables the person doing the layout to see how much space
will be required, and where the image will reside. 

One reason for doing this would be to conserve memory. Another is to
make it fast to render draft pages.

### Protection Proxy

Specifically oriented towards *controlling* access to the `Subject`.
Useful when we want to add the notion of *permissions* or *limits* to an
object that doesn't already have them.

* For example, limiting the number of pages that are passed to a
  (normally unrestricted) `PrintQueue` object. 

### Smart references

* replacement for "naked pointers"
  - can perform additional actions when an object is accessed
  - can be used to share a single object and make it appear as if there
    appeared to be many (all with the same initial state).
    * when a mutator is called on the proxy, it clones the shared state
      and modifies the cloned data (thus implementing copy-on-write
semantics)
  - can be used to maintain a count of the number of things referencing
    the shared object
    * when the count reaches 0 (but not before), the object can be
      safely deallocated
  - can be used to load a persistent object into memory when it is first
    used, as opposed to when it is first referenced
  - can be used to provide synchronisation: ensures the object is
    "locked" before access is granted (to prevent concurrent access) 

#### Note: copy-on-write semantics

* Can significantly reduce the cost of "copying" heavy-weight objects,
because the work is deferred until absolutely necessary. 
  - this technique is widely used by uber-pro hacker types (especially
    in languages like C++)
  
