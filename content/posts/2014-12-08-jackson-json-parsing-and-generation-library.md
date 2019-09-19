---
title: "Jackson: JSON parsing and generation library"
date: "2014-12-08"
template: "post"
draft: false
slug: "/posts/jackson-json-parsing-and-generation-library/"
category: "java"
tags:
  - "java"
description: ""
socialImage: ""
---

### 使用Jackson

+ 加入`jar` 包：
  - `jackson-annotation-2.2.3.jar`
  - `jackson-core-2.2.3.jar`
  - `jackson-databind-2.2.3.jar`
+ 创建 `com.fasterxml.jackson.databind.ObjectMapper`
+ 调用`ObjectMapper` 的 `writeValueAsString` 方法把`Java`对象或集合转为`json`字符串.
+ `Jackson` 根据`getter`来定位`Java`对象的属性,而不是字段
+ 可以在`getter`方法上添加注解: `@JsonIgnore`, 在转为`json`对象时忽略该属性.