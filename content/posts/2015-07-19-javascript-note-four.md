---
title: "JavaScript Note Four"
date: "2015-07-19"
template: "post"
draft: false
slug: "/posts/javascript-note-four/"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Chapter Five: Higher-order functions

+ Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.
+ Higher-order functions allow us to abstract over actions, not just values.

```javascript
function transparentWrapping(f) {
  return function() {
      return f.apply(null, arguments);
  };
}
```
+ The function it returns passes all of the given arguments, and only those arguments, to f.

#### JSON

+ JavaScript gives us functions, `JSON.stringify` and `JSON.parse`, that convert data from and to this format.

```javascript
var string = JSON.stringify({name: "X", born: 1980});
console.log(string);
// → {"name":"X","born":1980}
console.log(JSON.parse(string).born);
// → 1980
```

```javascript
function reduce(array, combine, start) {
  var current = start;
  for (var i = 0; i < array.length; i++)
      current = combine(current, array[i]);
  return current;
}
```

+ The standard array method `reduce` has an added convenience. If your array contains at least one element, you are allowed to leave off the start argument. The method will take the first element of the array as its start value and start reducing at the second element.

### Binding

+ The `bind` method, which all functions have, creates a new function that will call the original function but with some of the arguments already fixed.

```javascript
var theSet = ["Carel Haverbeke", "Maria van Brussel",
              "Donald Duck"];
function isInSet(set, person) {
  return set.indexOf(person.name) > -1;
}

console.log(ancestry.filter(function(person) {
  return isInSet(theSet, person);
}));
// → [{name: "Maria van Brussel", …},
//    {name: "Carel Haverbeke", …}]
console.log(ancestry.filter(isInSet.bind(null, theSet)));
// → … same result
```

#### Summary

+ Arrays provide a number of useful higher-order methods `forEach` to do something with each element in an array, `filter` to build a new array with some elements filtered out, `map` to build a new array where each element has been put through a function, and `reduce` to combine all an array’s elements into a single value.
+ Functions have an `apply` method that can be used to call them with an array specifying their arguments. They also have a `bind` method, which is used to create a partially applied version of the function.
