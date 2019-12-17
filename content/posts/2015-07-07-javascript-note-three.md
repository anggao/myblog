---
title: "JavaScript Note Three"
date: "2015-07-07"
template: "post"
draft: false
slug: "javascript-note-three"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Chapter Four: Data Structures- Objects and Arrays

- Properties that contain functions are generally called methods of the value they belong to.
- Some methods that array objects have

```javascript
var mack = [];
mack.push("Mack");
mack.push("the", "Knife");
console.log(mack);
// → ["Mack", "the", "Knife"]
console.log(mack.join(" "));
// → Mack the Knife
console.log(mack.pop());
// → Knife
console.log(mack);
// → ["Mack", "the"]
```

- Adding and removing things at the start of an array are called `unshift` and `shift`.
- The `indexOf` method has a sibling called `lastIndexOf`, which starts searching for the given element at the end of the array instead of the front.
- Another fundamental method is `slice`, which takes a start index and an end index and returns an array that has only the elements between those indices. The start index is inclusive, the end index exclusive.
- The `concat` method can be used to glue arrays together, similar to what the `+` operator does for strings.

```javascript
function remove(array, index) {
  return array.slice(0, index).concat(array.slice(index + 1));
}
console.log(remove(["a", "b", "c", "d", "e"], 2));
// → ["a", "b", "d", "e"]
```

- Reading a property that doesn’t exist will produce the value `undefined`.
- The `delete` operator when applied to a property access expression, will remove the named property from the object.

#### Mutability

- Numbers, strings, and Booleans, are all immutable—it is impossible to change an existing value of those types.
- Values of type string, number, and Boolean are not objects, and though the language doesn’t complain if you try to set new properties on them, it doesn’t actually store those properties. The values are immutable and cannot be changed.

#### Strings and their properties

- The `trim` method removes whitespace (spaces, newlines, tabs, and similar characters) from the start and end of a string.

#### The arguments object

- Whenever a function is called, a special variable named `arguments` is added to the environment in which the function body runs.
- In JavaScript you are allowed to **pass more (or fewer)** arguments to a function than the number of parameters the function itself declares.
  The `arguments` object has a `length` property that tells us the number of arguments that were really passed to the function.
  If that sounds a lot like an array to you, you’re right, it is a lot like an array. But this object, unfortunately, does not have any array methods (like `slice` or `indexOf`), so it is a little harder to use than a real array.

```javascript
function argumentCounter() {
  console.log("You gave me", arguments.length, "arguments.");
}
argumentCounter("Straw man", "Tautology", "Ad hominem");
// → You gave me 3 arguments.
```

#### The global object

- The global scope, the space in which global variables live, can also be approached as an object in JavaScript. Each global variable is present as a property of this object. In browsers, the global scope object is stored in the `window` variable.
