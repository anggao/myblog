---
title: "Design Pattern: Singleton"
date: "2013-05-22"
template: "post"
draft: false
slug: "design-pattern-singleton"
category: "Design Pattern"
tags:
  - "java"
description: ""
socialImage: ""
---

Singleton pattern ensures that there is only one instance of a class is created
in JVM. It is used to provide global point of access to the object.

###Implementation example: Lazy initialization

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null)
            instance = new Singleton();
        return instance;
    }
}
```

###Implementation example: Eager initialization

```java
public class Singleton {
    private static Singleton instance = new Singleton()

    private Singleton(){
    }

    public static Singleton getInstance(){
        return instance;
    }
}
```

In the second implementation, the Singleton instance is instantiated when the
class is loaded, while in the first implementation, the instance is not instantiated until
it is acturally needed, but the first implementation is not thread safe.
