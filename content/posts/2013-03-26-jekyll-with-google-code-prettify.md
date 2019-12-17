---
title: "Highlight Code With Google-code-prettify In Jekyll"
date: "2013-03-26"
template: "post"
draft: false
slug: "google-code-prettify"
category: "jekyll"
tags:
  - "jekyll"
  - "google-code-prettify"
description: ""
socialImage: ""
---

###[原文地址](http://blog.evercoding.net/2013/02/27/highlight-code-with-google-code-prettify/)

[google-code-prettfy](https://code.google.com/p/google-code-prettify/)，
效果不错, 最重要的是，非常小，加载速度比 SyntaxHighlighter 快得多，
而且，可以直接使用 markdown 的语法去写代码。

首先需要两个文件，prettify.js 和 prettify.css，自己去官网去下。把这两个放到模板中，如下

```javascript
<link href="/assets/themes//google-code-prettify/desert.css" rel="stylesheet" type="text/css" media="all">
<script type="text/javascript" src="/assets/themes//google-code-prettify/prettify.js"></script>
```

考虑到加载速度，最好 js 写到文档末尾，body 闭合标签之前，css 写到头部
之后，还需要加上如下代码，用于识别并高亮代码块

```javascript
$(function() {
  window.prettyPrint && prettyPrint();
});
```

这个需要使用 jQuery, 下面用 Google CDN:

```javascript
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
```

现在，就可以使用`<pre></pre>`标签进行高亮了，

```javascript
<pre class="prettyPrint">// code here</pre>
```

但这样会有些问题，就是在书写 html 代码的时候，html 标签会被浏览器认为是标签而不是代码的字符。
而 markdown 的语法写的代码其实已经解决了这个问题，所以，我们可以利用如下的 js 代码，
来避免自己用`<pre></pre>`写代码所出现的问题，同样需要 jQuery 支持

```javascript
$(function() {
  $("pre")
    .addClass("prettyprint linenums")
    .attr("style", "overflow:auto");
});
```

这样之后，就没有问题了，可以直接用 markdown 的前置 4 空格来写代码了。其中 addClass('prettyprint linenums')的 linenums 是添加行号的意思。
默认只显示第 5、10、15...行，可以在 css 文件中 li 的格式添加 list-style-type: decimal;，以显示全部行号

另外，如果博客中有用 bootstrap，其中对 pre 有如下几句

```css
white-space: pre;
white-space: pre-wrap;
word-break: break-all;
word-wrap: break-word;
```

这会使得 pre 中的代码自动换行，而不是溢出形成滚动条。如果不希望如此，可以注释掉。就看个人的喜好了。如果是滚动条，默认的滚动太难看，
可以修改一下样式([参考](http://www.javascript100.com/?p=756)):

```css
/* 设置滚动条的样式 */
::-webkit-scrollbar {
  width: 12px;
}
/* 滚动槽 */
::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}
/* 滚动条滑块 */
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background: rgba(0, 0, 0, 0.1);
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
}
::-webkit-scrollbar-thumb:window-inactive {
  background: rgba(255, 0, 0, 0.4);
}
```
