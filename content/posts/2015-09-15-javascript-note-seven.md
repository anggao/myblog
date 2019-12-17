---
title: "JavaScript Note Seven"
date: "2015-09-15"
template: "post"
draft: false
slug: "javascript-note-seven"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Modules

- Though JavaScript provides no actual module construct yet, objects can be used to create publicly accessible subnamespaces, and functions can be used to create an isolated, private namespace inside of a module.
- Functions are the only things in JavaScript that create a new scope.

```javascript
var dayName = (function() {
  var names = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday"
  ];
  return function(number) {
    return names[number];
  };
})();

console.log(dayName(3));
// → Wednesday
```

#### Objects as interfaces

```javascript
var weekDay = (function() {
  var names = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday"
  ];
  return {
    name: function(number) {
      return names[number];
    },
    number: function(name) {
      return names.indexOf(name);
    }
  };
})();

console.log(weekDay.name(weekDay.number("Sunday")));
// → Sunday
```

- A convenient alternative is to declare an object (conventionally named exports) and add properties to that whenever we are defining something that needs to be exported.

```javascript
(function(exports) {
  var names = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday"
  ];

  exports.name = function(number) {
    return names[number];
  };
  exports.number = function(name) {
    return names.indexOf(name);
  };
})((this.weekDay = {}));

console.log(weekDay.name(weekDay.number("Saturday")));
// → Saturday
```

- The module will claim a single global variable and wrap its code in a function in order to have its own private namespace. But this pattern still causes problems if multiple modules happen to claim the same name or if you want to load two versions of a module alongside each other.

#### CommonJS modules

- we can create a system that allows one module to directly ask for the interface object of another module, without going through the global scope.
- Our goal is a require function that, when given a module name, will load that module’s file (from disk or the Web, depending on the platform we are running on) and return the appropriate interface value.
- a function `readFile`, which returns the content of a given file as a string.
- execute this string as JavaScript code

#### Evaluating data as code

- special operator `eval`, which will execute a string of code in the current scope. This is usually a bad idea because it breaks some of the sane properties that scopes normally have, such as being isolated from the outside world.
- A better way of interpreting data as code is to use the Function constructor.
- This takes two arguments: a string containing a comma-separated list of argument names and a string containing the function’s body.

```javascript
function require(name) {
  var code = new Function("exports", readFile(name));
  var exports = {};
  code(exports);
  return exports;
}

console.log(require("weekDay").name(1));
// → Monday
```

- This removes a lot of clutter from our example module.

```javascript
var names = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday"
];

exports.name = function(number) {
  return names[number];
};
exports.number = function(name) {
  return names.indexOf(name);
};
```

- When using this pattern, a module typically starts with a few variable declarations that load the modules it depends on.

```javascript
var weekDay = require("weekDay");
var today = require("today");

console.log(weekDay.name(today.dayNumber()));
```

- The simplistic implementation of require given previously has several problems
- it will load and run a module every time it is required
- it is not possible for a module to directly export a value other than the exports object, such as a function

```javascript
function require(name) {
  if (name in require.cache) return require.cache[name];

  var code = new Function("exports, module", readFile(name));
  var exports = {},
    module = { exports: exports };
  code(exports, module);

  require.cache[name] = module.exports;
  return module.exports;
}
require.cache = Object.create(null);
```

#### Slow-loading modules

- Though it is possible to use the CommonJS module style when writing JavaScript for the browser, it is somewhat involved. The reason for this is that reading a file (module) from the Web is a lot slower than reading it from the hard disk
- One way to work around this problem is to run a program like `Browserify` on your code before you serve it on a web page.
- Another solution is to wrap the code that makes up your module in a function so that the module loader can first load its dependencies in the background and then call the function, initializing the module, when the dependencies have been loaded. That is what the `Asynchronous Module Definition (AMD)` module system does.

```javascript
define(["weekDay", "today"], function(weekDay, today) {
  console.log(weekDay.name(today.dayNumber()));
});

define([], function() {
  var names = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday"
  ];
  return {
    name: function(number) {
      return names[number];
    },
    number: function(name) {
      return names.indexOf(name);
    }
  };
});
```

#### minimal implementation of define

```javascript
var defineCache = Object.create(null);
var currentMod = null;

function getModule(name) {
  if (name in defineCache) return defineCache[name];

  var module = { exports: null, loaded: false, onLoad: [] };
  defineCache[name] = module;
  backgroundReadFile(name, function(code) {
    currentMod = module;
    new Function("", code)();
  });
  return module;
}
```

- The `define` function itself uses `getModule` to fetch or create the module objects for the current module’s dependencies.
- Its task is to schedule the `moduleFunction` (the function that contains the module’s actual code) to be run whenever those dependencies are loaded.

```javascript
function define(depNames, moduleFunction) {
  var myMod = currentMod;
  var deps = depNames.map(getModule);

  deps.forEach(function(mod) {
    if (!mod.loaded) mod.onLoad.push(whenDepsLoaded);
  });

  function whenDepsLoaded() {
    if (
      !deps.every(function(m) {
        return m.loaded;
      })
    )
      return;

    var args = deps.map(function(m) {
      return m.exports;
    });
    var exports = moduleFunction.apply(null, args);
    if (myMod) {
      myMod.exports = exports;
      myMod.loaded = true;
      myMod.onLoad.forEach(function(f) {
        f();
      });
    }
  }
  whenDepsLoaded();
}
```

- There are two popular, well-defined approaches to such modules.
- CommonJS Modules and revolves around a require function that fetches a module by name and returns its interface.
- AMD and uses a define function that takes an array of module names and a function and, after loading the modules, runs the function with their interfaces as arguments.
