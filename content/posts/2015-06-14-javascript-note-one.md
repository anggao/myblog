---
title: "JavaScript Note One"
date: "2015-06-14"
template: "post"
draft: false
slug: "javascript-note-one"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Chapter One: Values, Types, and Operators

#### Values

- There are six basic types of values in JavaScript:
  - numbers
  - strings
  - Booleans
  - objects
  - functions
  - undefined values

#### Numbers

- JavaScript uses a fixed number of bits, namely `64` of them, to store a single number value.
- For very big or very small numbers, you can also use scientific notation by adding an `e` (for "exponent"), followed by the exponent of the number: `2.99e8`
- Treat fractional digital numbers as approximations, not as precise values

#### Special numbers

- There are `three` special values in JavaScript that are considered numbers but don’t behave like normal numbers
  - Infinity
  - -Infinity
  - NaN
- Don’t put too much trust in infinity-based computation. It isn’t mathematically solid, and it will quickly lead to: `NaN`

#### Strings

- Strings cannot be divided, multiplied, or subtracted, but the + operator can be used on them. It does not add, but it `concatenates`

#### Unary operators

- One example is the `typeof` operator

#### Boolean values

- There is `only one` value in JavaScript that is `not equal to itself`, and that is `NaN`, which stands for "not a number".

#### Undefined values

- There are two special values, written `null` and `undefined`, that are used to denote the absence of a meaningful value.
- The difference in meaning between `undefined` and `null` is an accident of JavaScript’s design, and it doesn’t matter most of the time. In the cases where you actually have to concern yourself with these values, I recommend treating them as interchangeable (more on that in a moment).

#### Automatic type conversion

- When an operator is applied to the “wrong” type of value, JavaScript will quietly convert that value to the type it wants, using a set of rules that often aren’t what you want or expect. This is called `type coercion`.
- When null or undefined occurs on either side of the `==` operator, it produces true only if both sides are one of null or undefined.

```go
console.log(null == undefined);
// → true
```

- There are two extra operators: `===` and `!==`. The first tests whether a value is precisely equal to the other, and the second tests whether it is not precisely equal
