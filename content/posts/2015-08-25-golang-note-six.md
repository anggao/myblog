---
title: "Golang Note Six"
date: "2015-08-25"
template: "post"
draft: false
slug: "golang-note-six"
category: "golang"
tags:
  - "golang"
description: ""
socialImage: ""
---

- There are two types of Go programs: `executables` and `libraries`.
- Executable applications are the kinds of programs that we can run directly from the terminal
- Libraries are collections of code that we package together so that we can use them in other programs

### Types

#### Numbers

- Go has several different types to represent numbers. Generally we split numbers into two different kinds: integers and floating-point numbers.
- Go's integer types are: `uint8, uint16, uint32, uint64, int8, int16, int32 and int64`. 8, 16, 32 and 64 tell us how many bits each of the types use.
- there are two alias types: `byte` which is the same as `uint8` and `rune` which is the same as `int32`.
- there are also 3 machine dependent integer types: `uint, int and uintptr`
- Generally if you are working with integers you should just use the int type.
- Using a larger sized floating point number increases it's precision. (how many digits it can represent)
- Go has two floating point types: `float32` and `float64`
- two additional types for representing complex numbers (numbers with imaginary parts): `complex64` and `complex128`
- Generally we should stick with `float64` when working with floating point numbers.

#### Strings

- String literals can be created using double quotes `"Hello World"` or back ticks \`Hello World\`.

#### Booleans

- `true false`

#### Variables

- `var x string = "Hello World"`
- Since creating a new variable with a starting value is so common Go also supports a shorter statement
- `x := "Hello World"`
- Generally you should use this shorter form whenever possible.

#### Scope

- Go is lexically scoped using blocks
- Basically this means that the variable exists within the nearest curly braces `{ }` (a block) including any nested curly braces (blocks), but not outside of them.

#### Constants

- `const x string = "Hello World"`

#### Defining Multiple Variables

- Use the keyword `var` (or `const`) followed by parentheses with each variable on its own line.

```go
var (
  a = 5
  b = 10
  c = 15
)
```

#### Control Structures

```go
i := 1
for i <= 10 {
  fmt.Println(i)
  i = i + 1
}
```

```go
for i := 1; i <= 10; i++ {
  fmt.Println(i)
}
```

#### Arrays, Slices and Maps

##### Arrays

- An array is a numbered sequence of elements of a single type with a fixed length
- Go also provides a shorter syntax for creating arrays

```go
x := [5]float64{ 98, 93, 77, 82, 83 }
```

##### Slices

- Like arrays slices are indexable and have a length. Unlike arrays this length is allowed to change.
- If you want to create a slice you should use the built-in make function

```go
var x []float64
x := make([]float64, 6)
```

- This creates a slice that is associated with an underlying `float64` array of length 5. Slices are always associated with some array
- `x := make([]float64, 5, 10)` 10 represents the capacity of the underlying array which the slice points to
- Slice Functions
  - `append` creates a new slice by taking an existing slice (the first argument) and appending all the following arguments to it.
  - `copy` : copy(slice2, slice1)

##### Maps

- Maps have to be initialized before they can be used
- We can also delete items from a map using the built-in delete function

```go
x := make(map[string]int)
x["key"] = 10
delete(x, "key")
name, ok := elements["Un"]
if name, ok := elements["Un"]; ok {
  fmt.Println(name, ok)
}
```

- Like with arrays there is also a shorter way to create maps

```go
  elements := make(map[string]string){
    "A" : "a",
  }
```

- `map[string]map[string]string` a map of strings to maps of strings to strings

##### Functions

- `panic` which causes a run time error
- We can also name the return type
- Go is also capable of returning multiple values from a function
- Multiple values are often used to return an error value along with the result (`x, err := f()`), or a boolean to indicate success (`x, ok := f()`)
- There is a special form available for the last parameter in a Go function

```go
func Println(a ...interface{}) (n int, err error)

func add(args ...int) int {
}
xs := []int{1,2,3}
fmt.Println(add(xs...))
```

##### Closure

- It is possible to create functions inside of functions

```go
func main() {
  x := 0
  increment := func() int {
    x++
    return x
  }
  fmt.Println(increment())
  fmt.Println(increment())
}
```

- A function like this together with the non-local variables it references is known as a closure. In this case increment and the variable x form the closure.
- increment has type `func(int) int`

##### Defer, Panic & Recover

- `defer` which schedules a function call to be run after the function complete
- `defer` is often used when resources need to be freed in some way. For example when we open a file we need to make sure to close it later. With defer:

```go
f, _ := os.Open(filename)
defer f.Close()
```

- `panic` function causes a run time error. We can handle a run-time panic with the built-in `recover` function
- `recover` stops the `panic` and returns the value that was passed to the call to panic

```go
func main() {
  defer func() {
    str := recover()
    fmt.Println(str)
  }()
  panic("PANIC")
}
```

##### Pointers

- In Go a pointer is represented using the `*` (asterisk) character followed by the type of the stored value.
- `*` is also used to “dereference” pointer variables.
- `new` takes a type as an argument, allocates enough memory to fit a value of that type and returns a pointer to it

##### Structs and Interfaces

- A `struct` is a type which contains named fields.
- The `type` keyword introduces a new type. It's followed by the name of the type

```go
type Circle struct {
  x, y, r float64
}

// Initialization
var c Circle

// returns a *Circle
c := new(Circle)

// with init values
c := Circle{x: 0, y: 0, r: 5}

//  if we know the order fields
c := Circle{0, 0, 5}
```

- godoc fmt Println
