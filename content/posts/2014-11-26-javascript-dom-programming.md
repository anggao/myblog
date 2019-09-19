---
title: "JavaScript DOM Programming"
date: "2014-11-26"
template: "post"
draft: false
slug: "/posts/javascript-dom-programming/"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### 节点类型

+ 元素节点
+ 属性节点
  - 通常情况下，操作属性节点直接通过“元素节点.属性名”的方式来读写属性值，而不是获取属性节点。
+ 文本节点

### 获取元素节点

+ `document.getElementById`
+ `document.getElementsByTagName`
  - 并非`document` 对象独有
+ `document.getElementsByName`
  - 若`HTML` 元素本身没有`name` 属性，我们添加一个，使用这个方法对于IE不好使

### 获取子节点

+ `childNodes`
+ `firstChild`
+ `lastChild`
+ `getElementsByTagName`

### 读写文本节点

+ 文本节点一定是元素节点的子节点
+ 步骤：
  1. 获取文本几点所在的元素节点
  2. 利用firstChild 获取文本节点
  3. 利用节点的nodeValue 属性来读写文本值

### 所以节点都有的属性

+ `nodeType 1, 2, 3` 只读属性
+ `nodeName` 返回对应节点的名字，只读属性
+ `nodeValue null`, 属性值， 文本值, 可读写的属性

### 去除string 前后空格

```javascript
var reg = /^\s*|\s*$/g nameval = nameval.replace(reg, "");
```

### 创建节点/操作节点

+ 创建元素节点
  - `document.createElement(tagName)`
+ 创建属性节点
  - 先创建元素节点，使用.的方式为其属性赋值即可
+ 创建文本节点
  - `document.createTextNode(textContent)`
+ 新创建的节点只存在于内存中， 需要 `appendChild(newChild)`
+ 节点替换 `replaceChild(newChild, oldChild)`
  - 该方法除了替换外，还有移动节点的功能
  - `oldChild` 被替换为 `newChild, newChild` 被移动(删除)
+ 移除节点 `removeChild(refChild)`
+ 节点互换

```javascript
var replaceEach = function(aNode, bNode) {
    var aParent = aNode.parentNode;
    var bParent = bNode.parentNode;

    if (aParent && bParent) {
        var aNodeClone = aNode.cloneNode(true);
        bParent.replaceChild(aNodeClone, bNode);
        aParent.replaceChild(bNode, aNode);
    }
}
```

+ 节点的插入 `insertBefore(newNode, refNode)`
+ 自定义：`insertAfter(newNode, refNode)`

```javascript
var insertAfter = function(newNode, refNode) {
    var parentNode = refNode.parentNode;
    if (parentNode) {
        var lastNode = parentNode.lastChild;
        if (refNode == lastNode) {
            parentNode.appendChild(newNode);
        }
        else {
            var nextNode = refNode.nextSibling;
            parentNode.insertBefore(newNode, nextNode);
        }

    }
}
```

### innetHTML 属性
+ 非标准，读写属性