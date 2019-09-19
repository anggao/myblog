---
title: "JavaScript Note Nine"
date: "2015-09-19"
template: "post"
draft: false
slug: "/posts/javascript-note-nine/"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### The Document Object Model
+ scans a document for text nodes containing a given string and returns true when it has found one

```javascript
function talksAbout(node, string) {
  if (node.nodeType == document.ELEMENT_NODE) {
    for (var i = 0; i < node.childNodes.length; i++) {
      if (talksAbout(node.childNodes[i], string))
        return true;
    }
    return false;
  } else if (node.nodeType == document.TEXT_NODE) {
    return node.nodeValue.indexOf(string) > -1;
  }
}

console.log(talksAbout(document.body, "book"));
// → true
```

```javascript
<p data-classified="secret">The launch code is 00000000.</p>
<p data-classified="unclassified">I have two feet.</p>

<script>
  var paras = document.body.getElementsByTagName("p");
  Array.prototype.forEach.call(paras, function(para) {
    if (para.getAttribute("data-classified") == "secret")
      para.parentNode.removeChild(para);
  });
</script>
```

+ The `offsetWidth` and `offsetHeight` properties give you the space the element takes up in pixels.
+ `clientWidth` and `clientHeight` give you the size of the space inside the element, ignoring border width

### Handling Events

```javascript
<button>Click me</button>
<p>No handler here.</p>
<script>
  var button = document.querySelector("button");
  button.addEventListener("click", function() {
    console.log("Button clicked.");
  });
</script>

+ At any point, an event handler can call the stopPropagation method on the event object to prevent handlers “further up” from receiving the event.
+ If the handler doesn’t want the normal behavior to happen, typically because it has already taken care of handling the event, it can call the preventDefault method on the event object.
+ For letter and number keys, the associated key code will be the Unicode character code associated with the (uppercase) letter or number printed on the key. The charCodeAt method on strings gives us a way to find this code.
```javascript
console.log("Violet".charCodeAt(0));
// → 86
console.log("1".charCodeAt(0));
// → 49
```

+ "keypress", which fires right after "keydown" (and repeated along with "keydown" when the key is held) but only for keys that produce character input. The charCode property in the event object contains a code that can be interpreted as a Unicode character code.

```javascript
<p>Focus this page and type something.</p>
<script>
  addEventListener("keypress", function(event) {
    console.log(String.fromCharCode(event.charCode));
  });
</script>
```

+ "mousedown" and "mouseup" events are similar to "keydown" and "keyup" and fire when the button is pressed and released.
+ After the "mouseup" event, a "click" event fires on the most specific node that contained both the press and the release of the button.
+ When an element gains focus, the browser fires a "focus" event on it. When it loses focus, a "blur" event fires. Unlike the events discussed earlier, these two events do not propagate
+ Like the focus-related events, `loading` events do not propagate.
+ When a page is closed or navigated away from (for example by following a link), a "beforeunload" event fires.
+ It is important to understand that even though events can fire at any time, no two scripts in a single document ever run at the same moment.
+ A `worker` is an isolated JavaScript environment that runs alongside the main program for a document and can communicate with it only by sending and receiving messages.
