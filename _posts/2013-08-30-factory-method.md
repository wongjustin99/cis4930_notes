---
layout: post 
title: More on Factory Method  
categories: notes
tags: homework factory
---
{% include JB/setup %}

# Administrivia 30 August 2013
* HW due on Wednesday, Sept 4

| *Methods*            									 | Returns a new object | Returns an instance of a type | Implemented by subclasses |
| ---------------------------------------| -------------------- | ----------------------------- | ------------------------- |
| BorderFactory.createEtchedBorder()     | ?   									| yes 													| no 												|
| Array.asList( T...a ) 								 | yes 									| yes 													| no 												|
| foo.toString() 												 | yes 									| no (string is a concrete class) | yes 										|
| collection.iterator() 						 	   | yes 									| yes (tree/array iterators are different) | yes 						|

`collection.iterator` is the only real FactoryMethod of the bunch!

# Structure & Participants 
*insert graph from phone here made in UMLet*

Instances of Concrete**Creator**X *make* and/or *use/work-with* instances of Concrete**Product**X
# HW 1
1. Explain how the *intent* of **FM** is different from **BorderFactory**'s intent.
(100 min, 400 max) *remember the deliverables format!*
2. In your own words [200, 400], *how does the FM promote loose coupling*?
3. Consider the Strings representing names in the form: 
  * "First Last"
  * "Last, First" <- Note the comma
 
Assume an operation **Name makeName( String name )** which returns 

> | Name 					|
> |---------------|
> | -first:String |
> | -last:String  |
> 
> |               |
> |---------------|
> | ... 					|
 
with the fields correctly initalised with either form on String. 
 
*Is this an example of FM?* [200,400] words

# Implementation
## 1. GoF canonical
There are two major variations
* the **Creator** provides a *default implementation* (which may be overridden)

~~~ java
public Product makeProduct() {
	return new DefaultProduct();
}
~~~

This style is useful when there is a *reasonable* choice for a default product class. Note: in this case, the **Creator** need not be an abstract class. 

* the **Creator** is abstract and declares an abstract method: 
` abstract public Product makeProduct(); `

In this case, the **ConcreteCreator** *must* implement the FM

Note: this is the preferred style when there is *not* a reasonable default product

## 2. Parameterised Factory (aka, Simple Factory)
Note: there are many who believe this is *not* an example of FM. It is often called **Simple Factory**, especially by non-believers

A *single* PFM (or SF) can create and return instances of many **ConcreteProduct** classes; the class of the object returned is based on the actual parameter supplied. 

### Creator implements PFM

~~~ java
 public Product makeProduct( TypeCode type ) {
 	if type.equals( TypeA ) return new ProductTypeA();
 	if type.equals( TypeB ) return new ProductTypeB();
 	
 	throw new NoSuchProductException( type );
 }
~~~

### _Sub_Creator re-implement PFM

~~~ java
public Product makeProduct( TypeCode type ) {
	if type.equals( TypeA ) return new ProductTypeA2();
	if type.equals( TypeC ) return new ProductTypeC();
	
	return super.makeProduct( type );
}
~~~

### Discussion on PFM
* The **SubCreator** can override the **CProduct** for a given TypeCode that is *already* being handled by the **Creator**. This can lead to confusion, or be really cool, depending on your perspective. 
* Use of a **TypeCode**-especially when implemented as a **String** or **int**-can easily avoid the compiler's type checking mechanism, and thus allow bugs to pass unnoticed that would otherwise be caught. 
* Unless there is some meaningful type common to *all* the **Cproduct** classes, then the return type devolves to **Product == Object**, which forces the *caller* to know the *actual* type returned for a particular TypeCode, so that the caller can cast (or otherwise treat the object preferentially) to its actual type [as **Object** doesn't declare many "interesting" methods]

#### (Also true for FM)
* The **Creator** is typically the FM's client! The **Creator** almost always contains code that calls the (P)FM. Specific **CCreators** are usually meant to work with specific CProducts-which is *why* the FM is overridden by the **CCreator** class . 
	* FM makes the relationship transparent
	* PFM says the **CCreator** may work with *many* different **CProduct** types and *must* make an explicit decision about which one to use. 
