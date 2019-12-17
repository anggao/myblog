---
title: "JavaScript Note Eight"
date: "2015-09-18"
template: "post"
draft: false
slug: "javascript-note-eight"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### Project: A Programming Language

- We will build a programming language called Egg.
- Everything in Egg is an expression.
- An expression can be a variable, a `number`, a `string`, or an `application`.
- Applications are used for function calls but also for constructs such as if or while.
- And since the syntax has no concept of a block, we need a `do` construct to represent doing multiple things in sequence.
- The data structure that the parser will use to describe a program will consist of expression objects, each of which has a type property indicating the kind of expression it is and other properties to describe its content.
- The `>(x, 5)` part of the program would be represented like this:

```javascript
{
  type: "apply",
  operator: {type: "word", name: ">"},
  args: [
    {type: "word", name: "x"},
    {type: "value", value: 5}
  ]
}
```

- This is the parser

```javascript
function parseExpression(program) {
  program = skipSpace(program);
  var match, expr;
  if ((match = /^"([^"]*)"/.exec(program)))
    expr = { type: "value", value: match[1] };
  else if ((match = /^\d+\b/.exec(program)))
    expr = { type: "value", value: Number(match[0]) };
  else if ((match = /^[^\s(),"]+/.exec(program)))
    expr = { type: "word", name: match[0] };
  else throw new SyntaxError("Unexpected syntax: " + program);

  return parseApply(expr, program.slice(match[0].length));
}

function skipSpace(string) {
  var first = string.search(/\S/);
  if (first == -1) return "";
  return string.slice(first);
}

function parseApply(expr, program) {
  program = skipSpace(program);
  if (program[0] != "(") return { expr: expr, rest: program };

  program = skipSpace(program.slice(1));
  expr = { type: "apply", operator: expr, args: [] };
  while (program[0] != ")") {
    var arg = parseExpression(program);
    expr.args.push(arg.expr);
    program = skipSpace(arg.rest);
    if (program[0] == ",") program = skipSpace(program.slice(1));
    else if (program[0] != ")") throw new SyntaxError("Expected ',' or ')'");
  }
  return parseApply(expr, program.slice(1));
}

function parse(program) {
  var result = parseExpression(program);
  if (skipSpace(result.rest).length > 0)
    throw new SyntaxError("Unexpected text after program");
  return result.expr;
}

console.log(parse("+(a, 10)"));
// → {type: "apply",
//    operator: {type: "word", name: "+"},
//    args: [{type: "word", name: "a"},
//           {type: "value", value: 10}]}
```

- The evaluator

```javascript
function evaluate(expr, env) {
  switch (expr.type) {
    case "value":
      return expr.value;

    case "word":
      if (expr.name in env) return env[expr.name];
      else throw new ReferenceError("Undefined variable: " + expr.name);
    case "apply":
      if (expr.operator.type == "word" && expr.operator.name in specialForms)
        return specialForms[expr.operator.name](expr.args, env);
      var op = evaluate(expr.operator, env);
      if (typeof op != "function")
        throw new TypeError("Applying a non-function.");
      return op.apply(
        null,
        expr.args.map(function(arg) {
          return evaluate(arg, env);
        })
      );
  }
}

var specialForms = Object.create(null);
```
