---
title: "JavaScript Note Two"
date: "2015-06-15"
template: "post"
draft: false
slug: "/posts/javascript-note-two/"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Chapter Two: Program Structure

+ Anything that produces a value is an expression in JavaScript
+ The function `Number` converts a value to a number.
+ The `isNaN` function is a standard JavaScript function that returns true only if the argument it is given is `NaN`.

### Chapter Three: Functions

+ Variables declared outside of any function are called `global`, because they are visible throughout the program.
+ Functions can be created inside other functions, producing several degrees of locality.
+ In JavaScript, functions are the only things that create a new scope.
+ The next version of JavaScript will introduce a `let` keyword, which works like `var` but creates a variable that is local to the enclosing block, not the enclosing function.
+ Function declarations are not part of the regular top-to-bottom flow of control. They are conceptually moved to the top of their scope and can be used by all the code in that scope.
+ JavaScript is extremely broad-minded about the number of arguments you pass to a function. If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters simply get assigned the value `undefined`.

#### Closure
+ Being able to reference a specific instance of local variables in an enclosing function—is called `closure`.
+ A function that “closes over” some local variables is called a closure.
