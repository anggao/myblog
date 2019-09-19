---
title: "JavaScript Note Eleven"
date: "2015-12-07"
template: "post"
draft: false
slug: "/posts/javascript-note-eleven/"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### HTTP

+ Sending a request

```javascript
var req = new XMLHttpRequest();
req.open("GET", "example/data.txt", false);
req.send(null);
consle.log(req.responseText);
// → This is the content of data.txt
console.log(req.status, req.statusText);
// → 200 OK
console.log(req.getResponseHeader("content-type"));
// → text/plain
```

+ If we pass true as the third argument to open, the request is asynchronous.
```javascript
var req = new XMLHttpRequest();
req.open("GET", "example/data.txt", true);
req.addEventListener("load", function() {
  console.log("Done:", req.status);
});
req.send(null);
```

+ HTTP sandboxing
    - Browsers protect us by disallowing scripts to make HTTP requests to other domains
    - Servers can include a header in their response to explicitly indicate to browsers that it is okay for the request to come from other domains: `Access-Control-Allow-Origin: *`

+ Promises
    - Promises wrap an asynchronous action in an object, which can be passed around and told to do certain things when the action finishes or fails.

```javascript
function get(url) {
    return new Promise(function(succeed, fail){
        var req = new XMLHttpRequest();
        req.open('GET', url, true);
        req.addEventListener('load', function(){
            if (req.status < 400)
                succeed(req.responseText);
            else
                fail(new Error('Request failed: ' + req.statusText));
        });
        req.addEventListener('error', function(){
            fail(new Error('Network error'));
        });
        req.send(null);
    });
}
```

+ The promise acts as a handle to the request’s outcome.

```javascript
get('example/data.text').then(function(text){
    console.log('data.text: ' + text);
}, function(error){
    console.log('Failed to fetch data.text: ' + error);
});
```

+ Calling `then` produces a new promise, whose result (the value passed to success handlers) depends on the return value of the first function we passed to then.

### Forms and Form Fields

+ We can control focus from JavaScript with the `focus` and `blur` methods.
+ The value in `document.activeElement` corresponds to the currently focused element.
+ HTML also provides the `autofocus` attribute, which produces the same effect but lets the browser know what we are trying to achieve.
+ Browsers traditionally also allow the user to move the focus through the document by pressing the Tab key.
+ We can influence the order in which elements receive focus with the `tabindex` attribute.
+ By default, most types of HTML elements cannot be focused. But you can add a `tabindex` attribute to any element, which will make it focusable.

### Storing data client-side

+ You can store string data in a way that survives page reloads by putting it in the `localStorage` object.
```javascript
localStorage.setItem("username", "marijn");
console.log(localStorage.getItem("username"));
// → marijn
localStorage.removeItem("username");
```

+ The `localStorage` and `sessionStorage` objects can be used to save information in a way that survives page reloads.
+ `localStorage` saves the data forever (or until the user decides to clear it)
+ `sessionStorage` saves it until the browser is closed.
