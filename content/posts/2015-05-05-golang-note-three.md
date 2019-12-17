---
title: "Golang Note Three"
date: "2015-05-05"
template: "post"
draft: false
slug: "golang-note-three"
category: "golang"
tags:
  - "golang"
description: ""
socialImage: ""
---

### Methods

- Go does not have classes. However, you can define methods on struct types.
- The `method receiver` appears in its own argument list between the func keyword and the method name.
- You can declare a method on any type that is declared in your package, not just struct types.
- You cannot define a method on a type from another package (including built in types).
- Methods can be associated with a named type or a pointer to a named type.
  - There are two reasons to use a pointer receiver.
  - Avoid copying the value on each method call (more efficient if the value type is a large struct)
  - The method can modify the value that its receiver points to.

### Interfaces

- An interface type is defined by a set of methods.
- A value of interface type can hold any value that implements those methods.
- One of the most ubiquitous interfaces is `Stringer` defined by the `fmt` package.
- A Stringer is a type that can describe itself as a string.

```go
type Stringer interface {
    String() string
}
```

### Errors

- Go programs express error state with error values.
- The `error` type is a built-in interface simliar to `fmt.Stringer`

```go
type error interface {
    Error() string
}
```

- Functions often return an `error` value, and calling code should handle errors by testing whether the error equals `nil`.

```go
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
}
fmt.Println("Converted integer:", i)
```

### Readers

- The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data.

```go
func (T) Read(b []byte) (n int, err error)
```

`Read` populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an `io.EOF` error when the stream ends.

### Web servers

- Package `http` serves HTTP requests using any value that implements `http.Handler`

```go
package http

type Handler interface {
    ServeHTTP(w ResponseWriter, r *Request)
}
```
