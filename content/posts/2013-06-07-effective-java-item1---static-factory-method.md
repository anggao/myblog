---
title: "Effective Java: Item 01   Static Factory Method"
date: "2013-06-07"
template: "post"
draft: false
slug: "design-pattern-static-factory-method"
category: "Design Pattern"
tags:
  - "java"
description: ""
socialImage: ""
---

A class can provide a public static factory method, which is simply a static method that returns an instance of the class.

Providing a static factory method instead of a public constructor has both advantages and disadvantages:

- One advantage of static factory methods is that, unlike constructors, they have names, it can highlight differences between overloaded constructors.
- A second advantage of static factory methods is that, unlike constructors, they are not required to create a new obj each time they're invoked.
- A third advantage of static factory methods is that, unlike constructors, they can return an object of any subtype of their return type. (ref: [EnumSet](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/6-b14/java/util/EnumSet.java))
- A fourth advantage of static factory methods is that they reduce the verbosity of creating parameterized type instance.
- The main disadvantage of providing only static factory method is that classes without public or protected constructors cannot be subclassed.(favor composition over inheritance)
- A second disadvantage of static factory methods is that they are not readily distinguishable from other static methods (name convention).

Code Examples:

```java
public static Boolean valueOF(boolean b){
        return b ? Boolean.True : Boolean.FALSE;
}

/* Reduce the verbosity of creating parameterized type instance */
Map<String, List<String>> m = new HashMap<String, List<String>>;

public static <K,V> HashMap<K,V> newInstance(){
        return new HashMap<K,V>();
}

Map<String, List<String>> m = OwnClass.newInstance();
```
