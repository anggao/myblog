---
title: "JavaScript Note Five"
date: "2015-08-10"
template: "post"
draft: false
slug: "javascript-note-five"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Chapter Six: The Secret Life of Objects

- Methods are simply properties that hold function values
- There is a method similar to `apply`, called `call`. It also calls the function it is a method of but takes its arguments normally, rather than as an array. Like `apply` and `bind`, `call` can be passed a specific `this` value.

```javascript
var fn = function(arg1, arg2) {
  var str = "<p>aap " + this.noot + " " + arg1 + " " + arg2 + "</p>";
  document.body.innerHTML += str;
};
var context = {
  noot: "noot"
};
var args = ["mies", "wim"];

// Calls a function with a given 'this' value and arguments provided individually.
// Support: everywhere
fn.call(context, args[0], args[1]);

// Calls a function with a given 'this' value and arguments provided as an array
//  (or an array like object).
// Support: everywhere
fn.apply(context, args);

// Creates a new function that, when called, has its 'this' keyword set to the
//  provided value, with a given sequence of arguments preceding any provided
//  when the new function was called.
// Support: ECMAScript >= 5 (thus >= IE9)
var boundFn1 = fn.bind(context, args[0], args[1]);
boundFn1();

// Same as bind()
// Support: same as your jQuery version, available since 1.4
var boundFn2 = $.proxy(fn, context, args[0], args[1]);
boundFn2();
```

#### Prototypes

- In addition to their set of properties, almost all objects also have a prototype.
- **A prototype is another object that is used as a fallback source of properties.**

```javascript
console.log(Object.getPrototypeOf({}) == Object.prototype);
// → true
console.log(Object.getPrototypeOf(Object.prototype));
// → null
```

- Many objects don’t directly have `Object.prototype` as their prototype, but instead have another object, which provides its own default properties.

```javascript
console.log(Object.getPrototypeOf(isNaN) == Function.prototype);
// → true
console.log(Object.getPrototypeOf([]) == Array.prototype);
// → true
```

- You can use `Object.create` to create an object with a specific prototype

```javascript
var protoRabbit = {
  speak: function(line) {
    console.log("The " + this.type + " rabbit says '" + line + "'");
  }
};
var killerRabbit = Object.create(protoRabbit);
killerRabbit.type = "killer";
killerRabbit.speak("SKREEEE!");
// → The killer rabbit says 'SKREEEE!'
```

#### Constructors

- In JavaScript, calling a function with the `new` keyword in front of it causes it to be treated as a constructor.
- The constructor will have its `this` variable bound to a fresh object, and unless it explicitly returns another object value, this new object will be returned from the call.

```javascript
function Rabbit(type) {
  this.type = type;
}

var killerRabbit = new Rabbit("killer");
var blackRabbit = new Rabbit("black");
console.log(blackRabbit.type);
// → black
```

- Constructors (in fact, all functions) automatically get a property named prototype, which by default holds a plain, empty object that derives from `Object.prototype`.
- To add a speak method to rabbits created with the Rabbit constructor, we can simply do this

```javascript
Rabbit.prototype.speak = function(line) {
  console.log("The " + this.type + " rabbit says '" + line + "'");
};
blackRabbit.speak("Doom...");
// → The black rabbit says 'Doom...'
```

- The new object’s prototype will be the object found in the prototype property of the constructor function.

#### Prototype interference

```javascript
// Delete the problematic property again
delete Object.prototype.nonsense;
```

- JavaScript distinguishes between enumerable and nonenumerable properties.
- All properties that we create by simply assigning to them are enumerable. The standard properties in Object.prototype are all nonenumerable, which is why they do not show up in such a for/in loop.
- It is possible to define our own nonenumerable properties

```javascript
Object.defineProperty(Object.prototype, "hiddenNonsense", {
  enumerable: false,
  value: "hi"
});
for (var name in map) console.log(name);
// → pizza
// → touched tree
console.log(map.hiddenNonsense);
// → hi
```

- Deal with in operator

```javascript
console.log(map.hasOwnProperty("toString"));
// → false
```

- Generally do

```javascript
for (var name in map) {
  if (map.hasOwnProperty(name)) {
    // ... this is an own property
  }
}
```

#### Prototype-less objects

```javascript
var map = Object.create(null);
map["pizza"] = 0.069;
console.log("toString" in map);
// → false
console.log("pizza" in map);
// → true
```

#### Getters and setters

```javascript
var pile = {
  elements: ["eggshell", "orange peel", "worm"],
  get height() {
    return this.elements.length;
  },
  set height(value) {
    console.log("Ignoring attempt to set height to", value);
  }
};

console.log(pile.height);
// → 3
pile.height = 100;
// → Ignoring attempt to set height to 100
```

- add getter or setter to a property

```javascript
Object.defineProperty(TextCell.prototype, "heightProp", {
  get: function() {
    return this.text.length;
  }
});

var cell = new TextCell("no\nway");
console.log(cell.heightProp);
```
