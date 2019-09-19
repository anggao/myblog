---
title: "Golang Note One"
date: "2015-04-20"
template: "post"
draft: false
slug: "/posts/golang-note-one/"
category: "golang"
tags:
  - "golang"
description: ""
socialImage: ""
---

### Basics

+ Every Go program is made up of packages.
+ Programs start running in package main.
+ factored import statement.

```go
import (
    "fmt"
    "math"
)
```
+ In Go, a name is exported if it begins with a capital letter.
+ Functions in Golang

```go
func add(x int, y int) int {
    return x + y
}
```

+ When two or more consecutive named function parameters share a type, you can omit the type from all but the last.
+ A function can return any number of results.
+ Go's return values may be named and act just like variables.
+ A return statement without arguments returns the current values of the results. This is known as a "naked" return.
+ Naked return statements should be used only in short function, as with the example shown here. They can harm readability in longer functions.

```go
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}
```

+ The var statement declares a list of variables; as in function argument lists, the type is last.
+ A var statement can be at package or function level.
+ If an initializer is present, the type can be omitted; the variable will take the type of the initializer.
+ Short variable declarations: inside a function, the := short assignment statement can be used in place of a var declaration with implicit type.
+ Outside a function, every statement begins with a keyword (var, func, and so on) and so the `:=` construct is not available.
+ Variable declarations may be "factored" into blocks, as with import statements.
+ Variables declared without an explicit initial value are given their zero value.
+ Type conversions: The expression `T(v)` converts the value v to the type T.
+ Type inference: When declaring a variable without specifying its type (using var without a type or the `:=` syntax), the variable's type is inferred from the value on the right hand side.
+ Constants are declared like variables, but with the const keyword.
+ Constants cannot be declared using the := syntax.
+ An untyped constant takes the type needed by its context.