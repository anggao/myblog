---
title: "Golang Note Two"
date: "2015-04-23"
template: "post"
draft: false
slug: "/posts/golang-note-two/"
category: "golang"
tags:
  - "golang"
description: ""
socialImage: ""
---

### Flow control statements

+ Go has only one looping construct, the for loop.
+ For is Go's "while"
+ Like `for`, the `if` statement can start with a short statement to execute before the condition.
+ A case body breaks automatically, unless it ends with a fallthrough statement.
+ A `defer` statement defers the execution of a function until the surrounding function returns.
+ The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.
+ Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

### Pointers

+ Go has pointers. A pointer holds the memory address of a variable.
+ The type `*T` is a pointer to a `T` value. Its zero value is `nil`.

```go
var p *int
```

+ The `&` operator generates a pointer to its operand.
+ The `*` operator denotes the pointer's underlying value.
+ Unlike C, Go has no pointer arithmetic.

### Struct

+ A `struct` is a collection of fields.
+ A `struct literal` denotes a newly allocated struct value by listing the values of its fields.

### Arrays

+ The type `[n]T` is an array of `n` values of type `T`.

```go
var a [10]int
```

+ An array's length is part of its type, so arrays cannot be resized.

### Slices
+ A slice points to an array of values and also includes a length.
+ `[]T` is a slice with elements of type `T`.
+ Slices can be re-sliced, creating a new slice value that points to the same array.

```go
// is empty
s[lo:lo]
// one element
s[lo:lo+1]
```

+ Slices are created with the `make` function. It works by allocating a zeroed array and returning a slice that refers to that array:

```go
a := make([]int, 5)  // len(a)=5
```

+ To specify a capacity, pass a third argument to make:

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

+ The zero value of a slice is `nil`.
+ A `nil` slice has a length and capacity of `0`.
+ The resulting value of `append` is a slice containing all the elements of the original slice plus the provided values.
+ If the backing array of s is too small to fit all the given values a bigger array will be allocated.

### Range

+ The range form of the for loop iterates over a slice or map.

### Maps

+ Maps must be created with `make` (not new) before use; the `nil` map is empty and cannot be assigned to.
+ Delete an element: `delete(m, key)`
+ Test that a key is present with a two-value assignment: `elem, ok = m[key]` If `key` is in `m`, `ok` is `true`. If not, `ok` is `false` and `elem` is the zero value for the map's element type.

### Function closures

+ Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.
