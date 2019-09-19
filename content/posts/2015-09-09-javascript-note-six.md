---
title: "JavaScript Note Six"
date: "2015-09-09"
template: "post"
draft: false
slug: "/posts/javascript-note-six/"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Exceptions

```javascript
try {
  (function() {
    throw new Error("Error !")
  })()
} catch (error) {
  console.log("Something went wrong: " + error);
} finally {
  console.log("finally called");
}
```


+ JavaScript (in a rather glaring omission) doesn’t provide direct support for selectively catching exceptions
+ We want to catch a specific kind of exception. We can do this by checking in the catch block whether the exception we got is the one we are interested in and by rethrowing it otherwise
+ let’s define a new type of error and use instanceof to identify it.

```javascript
function InputError(message) {
  this.message = message;
  this.stack = (new Error()).stack;
}
InputError.prototype = Object.create(Error.prototype);
InputError.prototype.name = "InputError"

try {
  ...
} catch(e) {
  if (e instanceof InputError)
    console.log("InputError !");
  else
    throw e;
}
```

+ Assertions are a tool to do basic sanity checking for programmer errors.

```javascript
function AssertionFailed(Message) {
  this.message = message;
}
AssertionFailed.prototype = Object.create(Error.prototype);
function assert(test, message) {
  if (!test)
    throw new AssertionFailed(message);
}
```

### Regular Expressions

+ A regular expression is a type of object
+ It can either be constructed with the RegExp constructor or written as a literal value by enclosing the pattern in forward slash (`/`) characters.

```javascript
var re1 = new RegExp("abc");
var re2 = /abc/;
console.log(/abc/.test("abcde"));
// → true
console.log(/[0-9]/.test("in 1992"));
// → true
```

+ `\d` Any digit character, means the same thing as `[0-9]`.
+ `\w` An alphanumeric character (“word character”)
+ `\s` Any whitespace character (space, tab, newline, and similar)
+ `\D` A character that is not a digit
+ `\W` A nonalphanumeric character
+ `\S` A nonwhitespace character
+ `.` Any character except for newline

```javascript
var dateTime = /\d\d-\d\d-\d\d\d\d \d\d:\d\d/;
console.log(dateTime.test("30-01-2003 15:20"));
// → true
console.log(dateTime.test("30-jan-2003 15:20"));
// → false
var notBinary = /[^01]/;
console.log(notBinary.test("1100100010100110"));
// → false
console.log(notBinary.test("1100100010200110"));
// → true
```

+ These backslash codes can also be used inside square brackets. For example, `[\d.]` means any digit or a period character. But note that the period itself, when used between square brackets, loses its special meaning. The same goes for other special characters, such as `+.`

#### Repeating parts of a pattern

```javascript
var dateTime = /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/;
console.log(dateTime.test("30-1-2003 8:45"));
// → true
```

#### Grouping subexpressions

```javascript
var cartoonCrying = /boo+(hoo+)+/i;
console.log(cartoonCrying.test("Boohoooohoohooo"));
// → true
```

+ The `i` at the end of the expression in the previous example makes this regular expression case insensitive

#### Matches and groups

+ Regular expressions also have an `exec` (execute) method that will return `null` if no match was found and return an object with information about the match otherwise.

```javascript
var match = /\d+/.exec("one two 100");
console.log(match);
// → ["100"]
console.log(match.index);
// → 8
```

+ String values have a match method that behaves similarly.

```javascript
console.log("one two 100".match(/\d+/));
// → ["100"]
```

+ When the regular expression contains subexpressions grouped with parentheses, the text that matched those groups will also show up in the array.
+ When a group does not end up being matched at all (for example, when followed by a question mark), its position in the output array will hold undefined. Similarly, when a group is matched multiple times, only the last match ends up in the array.

```javascript
var quotedText = /'([^']*)'/;
console.log(quotedText.exec("she said 'hello'"));
// → ["'hello'", "hello"]
console.log(/bad(ly)?/.exec("bad"));
// → ["bad", undefined]
console.log(/(\d)+/.exec("123"));
// → ["123", "3"]
```

### The date type

+ JavaScript has a standard object type for representing dates—or rather, points in time. It is called `Date`.

```javascript
console.log(new Date());
// → Wed Dec 04 2013 14:24:57 GMT+0100 (CET)
console.log(new Date(2009, 11, 9));
// → Wed Dec 09 2009 00:00:00 GMT+0100 (CET)
console.log(new Date(2009, 11, 9, 12, 59, 59, 999));
// → Wed Dec 09 2009 12:59:59 GMT+0100 (CET)
```

+ JavaScript uses a convention where **month numbers start at zero (so December is 11)**, yet day numbers start at one. This is confusing and silly. Be careful.
+ The `getTime` method on a date object returns “Unix time”

```javascript
console.log(new Date(2013, 11, 19).getTime());
// → 1387407600000
console.log(new Date(1387407600000));
// → Thu Dec 19 2013 00:00:00 GMT+0100 (CET)

function findDate(string) {
  var dateTime = /(\d{1,2})-(\d{1,2})-(\d{4})/;
  var match = dateTime.exec(string);
  return new Date(Number(match[3]),
                  Number(match[2]) - 1,
                  Number(match[1]));
}
console.log(findDate("30-1-2003"));
// → Thu Jan 30 2003 00:00:00 GMT+0100 (CET)
```

+ If we want to enforce that the match must span the whole string, we can add the markers `^` and `$`.
+ If, on the other hand, we just want to make sure the date starts and ends on a word boundary, we can use the marker `\b`.

```javascript
console.log(/cat/.test("concatenate"));
// → true
console.log(/\bcat\b/.test("concatenate"));
// → false
```

+ The regular expression `/\b([01]+b|\d+|[\da-f]+h)\b/` matches either a binary number followed by a `b`, a regular decimal number with no suffix character, or a hexadecimal number (that is, base 16, with the letters a to f standing for the digits 10 to 15) followed by an `h`.

#### The replace method
+ String values have a replace method, which can be used to replace part of the string with another string.
+ The first argument can also be a regular expression, in which case the first match of the regular expression is replaced
+ When a `g` option (for global) is added to the regular expression, all matches in the string will be replaced, not just the first.

```javascript
console.log("Borobudur".replace(/[ou]/, "a"));
// → Barobudur
console.log("Borobudur".replace(/[ou]/g, "a"));
// → Barabadar

console.log(
  "Hopper, Grace\nMcCarthy, John\nRitchie, Dennis"
    .replace(/([\w ]+), ([\w ]+)/g, "$2 $1"));
// → Grace Hopper
//   John McCarthy
//   Dennis Ritchie
```

+ The `$1` and `$2` in the replacement string refer to the parenthesized groups in the pattern.
+ The whole match can be referred to with `$&`.
+ It is also possible to pass a function, rather than a string, as the second argument to replace. For each replacement, the function will be called with the matched groups (as well as the whole match) as arguments, and its return value will be inserted into the new string.

#### Greed
+ The repetition operators (`+, *, ?, and {}`) are greedy, meaning they match as much as they can and backtrack from there.
+ If you put a question mark after them (`+?, *?, ??, {}?`), they become nongreedy and start by matching as little as possible

#### Dynamically creating RegExp objects

```javascript
var name = "harry";
var text = "Harry is a suspicious character.";
var regexp = new RegExp("\\b(" + name + ")\\b", "gi");
console.log(text.replace(regexp, "_$1_"));
// → _Harry_ is a suspicious character.
```

+ Adding backslashes before alphabetic characters is a bad idea because things like `\b` and `\n` have a special meaning. But escaping everything that’s not alphanumeric or whitespace is safe.

```javascript
var name = "dea+hl[]rd";
var text = "This dea+hl[]rd guy is super annoying.";
var escaped = name.replace(/[^\w\s]/g, "\\$&");
var regexp = new RegExp("\\b(" + escaped + ")\\b", "gi");
console.log(text.replace(regexp, "_$1_"));
// → This _dea+hl[]rd_ guy is super annoying.
```

#### The search method

+ The `indexOf` method on strings cannot be called with a regular expression.
+ There is another method, `search`, which does expect a regular expression

```javascript
console.log("  word".search(/\S/));
// → 2
console.log("    ".search(/\S/));
// → -1
```

#### The lastIndex property
+ Regular expression objects have properties.
+ `source`, which contains the string that expression was created from
+ `lastIndex`, which controls, in some limited circumstances, where the next match will start
+ Those circumstances are that the regular expression must have the global (g) option enabled, and the match must happen through the `exec` method
+ If the match was successful, the call to `exec` automatically updates the `lastIndex` property to point after the match. If no match was found, `lastIndex` is set back to zero

#### Looping over matches

```javascript
var input = "A string with 3 numbers in it... 42 and 88.";
var number = /\b(\d+)\b/g;
var match;
while (match = number.exec(input))
  console.log("Found", match[1], "at", match.index);
// → Found 3 at 14
//   Found 42 at 33
//   Found 88 at 40
```

#### Parsing an INI file

```javascript
function parseINI(string) {
  // Start with an object to hold the top-level fields
  var currentSection = {name: null, fields: []};
  var categories = [currentSection];

  string.split(/\r?\n/).forEach(function(line) {
    var match;
    if (/^\s*(;.*)?$/.test(line)) {
      return;
    } else if (match = line.match(/^\[(.*)\]$/)) {
      currentSection = {name: match[1], fields: []};
      categories.push(currentSection);
    } else if (match = line.match(/^(\w+)=(.*)$/)) {
      currentSection.fields.push({name: match[1],
                                  value: match[2]});
    } else {
      throw new Error("Line '" + line + "' is invalid.");
    }
  });

  return categories;
}
```

#### International characters

+ Things like `é` or `β`, which most definitely are word characters, will not match `\w` (and will match uppercase `\W`, the nonword category).
+ By a strange historical accident, `\s` (whitespace) does not have this problem and matches all characters that the Unicode standard considers whitespace

#### Summary

```javascript
/abc/       A sequence of characters
/[abc]/   Any character from a set of characters
/[^abc]/    Any character not in a set of characters
/[0-9]/   Any character in a range of characters
/x+/        One or more occurrences of the pattern x
/x+?/       One or more occurrences, nongreedy
/x*/        Zero or more occurrences
/x?/        Zero or one occurrence
/x{2,4}/    Between two and four occurrences
/(abc)/   A group
/a|b|c/   Any one of several patterns
/\d/        Any digit character
/\w/        An alphanumeric character (“word character”)
/\s/        Any whitespace character
/./       Any character except newlines
/\b/        A word boundary
/^/       Start of input
/$/       End of input
```

A regular expression has a method test to test whether a given string matches it. It also has an exec method that, when a match is found, returns an array containing all matched groups. Such an array has an index property that indicates where the match started.

Strings have a match method to match them against a regular expression and a search method to search for one, returning only the starting position of the match. Their replace method can replace matches of a pattern with a replacement string. Alternatively, you can pass a function to replace, which will be used to build up a replacement string based on the match text and matched groups.

Regular expressions can have options, which are written after the closing slash. The i option makes the match case insensitive, while the g option makes the expression global, which, among other things, causes the replace method to replace all instances instead of just the first.

The RegExp constructor can be used to create a regular expression value from a string.