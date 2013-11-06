---
layout: post
title: "Term Project"
description: ""
category: notes
tags: [project, command, observer]
---
{% include JB/setup %}

# Administrivia

* Term project

# Term Project

## Requirements 

* Syntax highlighting
* suppport both mono and non-monospacing
* Auto-indentation
	* press tab one time, and comments line up on the end 
	* use tab key to set up formatting

* Variables lining up with types
* Font Metrics 
	- research this
* language interpretation
	- prase "std::cout << value" to use a chevon instead
	- `a[1][2]` would become `a`, with subscript `2,3`
	- `alpha = 7;`
		- "GREEK LETTER ALPHA" = 7;
		- for all greek characters
		- be able to have other languages besides C++
			- know keywords, etc. 
# Command (233) cont.

## A continuum of sophistication

The pattern allows for everything from a simple `Command` -- which binds a specific request to a specific procedure -- to very complex `Command`s -- which may involve multiple requests and multiple receivers. 

```
// Simple
app.openUsingDialogue()

// Complex
name = dialogue.selectFile();
file = new File( name );
doc = new Document( file );
app.addDocument( doc );

## Cool super powers

### Macros

* The pattern enables the creation of `Composite Commands` (aka, Macros)
	* multiple `Command` objects are aggregated and treated as a single `Command`
	* when `execute()`-ed, each of the `Command`s will be triggered in sequence.

#### Support for multi-level undoable/redoable operations

* See _Object-Oriented Software Construction, 2e_ Chapter 21, Section 3
	* Tour-De-Force of OOP, with Lance Armstrong

#### Deferred Execution

A `Command` need not be executed immediately; it can be queued for later execution

![Diagram of Players playing a game with an Executor]()

* Players are all putting their commands into a `Command` queue. 
* the `Executor` sits in a loop [or blocks until the queue is not empty]
	* it will remove the oldest item, and then execute it
* this is often done for online multiplayer games

#### Use in frameworks

* Enables one to implement a framework in which an *actor* triggers a *generic request*. 
	* the developer building an application using the framework would impliement a *specific request* as a `Command`. 

>Davis: This is just like the ActionListener from last lecture 
	> All the stuff is defined in the framework

* it makes it easy to add new `Command`s to the system. 

> Davis: We aren't limited to what the developer who made the fraemwork thought of, just what the developer *using* the framework thought of. 

# Observer (293)

## Intent

Define a one-to-many dependency between objects, so that when one object changes its state, all the dependents are notified and update automatically. 

## Initial Comments

By __dependent__, he doesn't mean "tightly coupled". What it does mean, is that when an `object A`'s state changes, `object B` (C, & D) may need to perform some action in response. 

![Diagram of a HFT system]()

the `Subject` allows `Observers` to dynamically attach/deattach themselves from the list of objects to be notified when `Subject`'s state changes. 

### What happens when the *Subject* has multiple state elements? 

An `Observer` may be interested in just a *subset* of the `Subject`'s state elements. 

#### Notification models

* push: the `Subject` includes __in the notification__ info about *what* has changed
* pull: the `Observer` must query the `Subject` to determine what has changed

#### An alternative to push/pull

* The `Subject` maintains multiple notification lists (one per state element)  
	* an `Observer` registers for only those elements of which it is actually interested
