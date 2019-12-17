---
title: "jQuery notes"
date: "2014-11-27"
template: "post"
draft: false
slug: "jquery-notes"
category: "javascript"
tags:
  - "javascript"
description: ""
socialImage: ""
---

### jQuery 对象 VS DOM 对象

- 两者不能互相使用对方属性和方法
- `jQuery` 对象时一个 DOM 数组对象,可以使用下标转为`DOM`对象
  - `var $btn = $("button");`
  - `var btn = $btn[0];`
- `jQuery` 对象是使用`$()`包装`DOM`对象后产生的对象

### jQuery 选择器

- 选取被选中的`select` 的`option` 需要使用选取子节点的方式
  - `$("select[name='test'] :selected").each(function({...}));`

### jQuery 对象的几个方法

- `val()` 获取或设置表单元素的`value`属性值
  - `$(":text:enabled").val('test');`
  - `$(":text:enabled").val();`
- `attr()`
  - `attr(name, val);`
  - `attr(name);`
- `each()` 对`jQuery` 对象进行遍历，其参数为`function`, 函数内部的`this` 是正在遍历的 DOM 对象
  - `$("select :selected").each(function(){alert(this.value;)});`
- `text()` 和`val()`类似,获取元素节点的文本子节点的值

### 基本选择器

- `#id`
- `.class`
- `element`
- `*`
- `selector1,selector2..` 并集
  - `$("span,#two").css("background", "red");`

### 层次选择器

- `ancestor descendant`
- `parent > child`
- `prev + next`
- `prev ~ siblings`
- **_注意_**: `(prev ~ div)` 选择器只能选择 `prev` 元素后面的同辈元素; 而 `jQuery` 中的方法 `siblings()` 与前后位置无关, 只要是同辈节点就可以选取
- 选择 `id` 为 `one` 的下一个 `span` 元素
  - 以下选择器选择的是近邻 `#one` 的 `span` 元素, 若该`span` 和 `#one` 不相邻, 选择器无效.
  - `$("#one + span")`
  - 正确写法：
  - `$("#one").nextAll("span:first")`
  - `$("#one ~ span").first()`
- 选择 `id` 为 `two` 的元素前边的所有的 `div` 兄弟元素
  - `$("#two").prevAll("div")`

### 过滤选择器

- 过滤选择器主要是通过特定的过滤规则来筛选出所需的 `DOM` 元素, 该选择器都以 : 开头
- 按照不同的过滤规则, 过滤选择器可以分为
  - 基本过滤
    - `:first`
    - `:last`
    - `:not(selector)`
    - `:even` 索引从 0 开始
    - `:header * :animated`
    - 等等...
  - 内容过滤
    - `:contains(text)`
    - `:empty`
    - `:has(selector)`
    - `:parent`
  - 可见性过滤
    - `:hidden`
    - `:visible`
  - 属性过滤
    - 属性过滤选择器的过滤规则是通过元素的属性来获取相应的元素
    - eg: 选取有属性 id 的 div 元素，然后在结果中 选取属性`title`值 含有`es`的 `div` 元素
      - `$("div[id][title*='es']")`
  - 子元素过滤
    - `:first-child`
    - `:last-child`
    - `:only-child`
    - `:nth-child(index/even/odd/equation)` 索引从 1 开始
      - `nth-child(3n + 1)`: 能选取每个父元素下的索引值是 `3n + 1`的元素
    - 选取每个`class`为`one`的`div`父元素下的第 2 个子元素.
      - `$("div.one :nth-child(2)")` 选取子元素, 需要在选择器前添加一个空格.
  - 表单对象属性过滤选择器
    - `:enabled`
    - `:disabled`
    - `:checked` 单选框，复选框
    - `:selected` 下拉列表

### 表单选择器

- `:text` 单行文本
- `:raido`
- `:checkbox`
- 等等..
- 对表单内 可用 input 赋值操作
  - `$(":text:enabled").val('test');`
- 获取多选框选中的内容
  - `$(":checkbox[name='test']:checked").each(function(){alert(this.value)})`
- 获取下拉框选中的内容

```javascript
//实际被选择的不是 select, 而是 select 的 option 子节点
//所以要加一个 空格.
$(":select :selected").each(function() {
  alert(this.value);
});
```

### 使用 jQuery 进行 DOM 操作

- 新建（元素，属性，文本）节点，直接使用`$()`包装即可，返回一个`jQuery`对象
- 把节点插入到文档中
  - `append, appendTo`
  - `prepend, prependTo`
  - `before, insertBefore`
  - `after, insertAfter`
- 删除节点: `remove`
- 清空节点: `empty`
- 克隆节点: `clone`
  - `clone()`: 克隆匹配的 `DOM` 元素, 返回值为克隆后的副本. 但此时复制的新节点不具有任何行为
  - `clone(true)`: 复制元素的同时也复制元素中的的事件
- 替换节点: `replaceWith, replaceAll`

- `defaultValue` 是`DOM`对象的属性，可以获取表单元素的默认值 ＋ 通过`val()`为`radio`赋值，即便是一组 radio 赋值，`val`参数也应该使用数组
- `val` 不能直接获取 `checkbox` 被选择的值,若直接获取, 只能得到第一个被选择的值. 因为 `$(":checkbox[name='c']:checked")` 得到的是一个数组. 而使用 `val()` 方法只能获取数组元素的第一个值,若希望打印被选择的所有值, 需要使用 `each` 遍历. `alert($(":checkbox[name='c']:checked").val());`
- 而 `raido` 被选择的只有一个, 所以可以直接使用 `val()` 方法.
- `$.trim(str)`: 可以去除 `str` 的前后空格
- `find()` 方法： 查找子节点，返回值为子节点对应的`jQuery`对象 `var textContent = $trNode.find("td:first").text()`

### jQuery 中的事件

- 合成事件
  - `hover()`: 模拟光标悬停事件. 当光标移动到元素上时, 会触发指定的第一个函数, 当光标移出这个元素时, 会触发指定的第二个函数
  - `toggle()`: 用于模拟鼠标连续单击事件. 第一次单击元素, 触发指定的第一个函数, 当再一次单击同一个元素时, 则触发指定的第二个函数, 如果有更多个函数, 则依次触发, 直到最后一个
  - `toggle()` 的另一个作用: 切换元素的可见状态
- 使用 `is()` 方法, 来判断某个给定的 jQuery 对象是否符合指定的选择器. `var flag = $(".content").is(":hidden");`

### jQuery 中的动画

- `hide()`: 在 `HTML` 文档中, 为一个元素调用 `hide()` 方法会将该元素的 `display` 样式改为 `none`代码功能同 `css(“display”, “none”)`
- `show()`: 将元素的 `display` 样式改为先前的显示状态
- `fadeIn(), fadeOut()`: 只改变元素的透明度
