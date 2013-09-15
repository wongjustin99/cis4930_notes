# Design Patterns vs. Frameworks

## Frameworks

* a reusable mini-architecture
* emphasizes _design_ reuse over _code_ reuse
  - reusing the framework's design (and implicitly its code), but not
    reusing *your* code. 
* the frameworks provide a hardcoded context of customisable hotspots
  - callbacks
  - (message forwarding)
  - subclassing

### Frameworks vs. Design Patterns
* Frameworks
  * Executable code
  * "physical"

* Design Pattern
  * Knowledge/experience about software
  * "conceptual"or "logical"

Frameworks are typically implemented using multiple design patterns.

# A brief Overview of some DP pros and cons

* Con: may require a much larger initial investment than an ad-hoc
  approach
* Con: adds complexity to a design
* Pro: adds flexibility
* Pro: increases reusability

# Overview of UML class diagram notation

see pdf online

# OO reuse mechanisms

There are three that are used widely...

1. Reuse via subclassing
  a. Glassbox inheritance (aka whitebox inheritance)
    * The subclass has knowledge of and access to (some of) the
      superclass'implemtnation detail (includes access *protected*
members) => evil, EVIL, EVIL!
  b. Blackbox inheritance
    * The subclass is written in terms of the superclass'(public)
      interface.
      * The Gang of Four seem to assume that this isn't possible, but
        with a more modern language, like Java, it most certainly is. =>
Good design practise!

Inheritance is a static relationship

* The relationship between super and subclass is established at
  compile-time and cannot be changed without recompilation. 

 
