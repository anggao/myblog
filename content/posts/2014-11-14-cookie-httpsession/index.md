---
title: "Cookie & HttpSession"
date: "2014-11-14"
template: "post"
draft: false
slug: "/posts/cookie-httpsession/"
category: "java"
tags:
  - "java"
description: ""
socialImage: ""
---

### Cookie

+ 完成会话跟踪的一种机制: 采用的是在客户端保持 HTTP 状态信息的方案
+ `Cookie`是在浏览器访问WEB服务器的某个资源时，由WEB服务器在HTTP响应消息头中附带传送给浏览器的一个小文本文件
+ 一旦WEB浏览器保存了某个`Cookie`，那么它在以后每次访问该WEB服务器时，都会在HTTP请求头中将这个Cookie回传给WEB服务器

![cookie.png](./cookie.png)

### Cookie 相关API

#### 写入Cookie

```java
//1. 创建一个Cookie对象
Cookie cookie = new Cookie("name", "ang");
//2. setMaxAge: 设置Cookie 的最大时效,以秒为单位，若为0，表示立即删除该Cookie
// 若为负数，表示不存储改Cookie
cookie.setMaxAge(30);
//3. 设置Cookie的作用范围
cookie.setPath(request.getContextPath())
//4. 调用response 的一个方法把Cookie传给客户端
response.addCookie(cookie);
```

#### 获取Cookie

```java
Cookie[] cookies = request.getCookies();
if(cookies != null && cookies.length > 0) {
    for (Cookie cookie: cookies)
        out.print(cookie.getName() + ": " + cookie.getValue());
}
```

### 会话cookie和持久cookie

+ 如果不设置过期时间，则表示这个cookie生命周期为浏览器会话期间，只要关闭浏览器窗口，cookie就消失了。这种生命期为浏览器会话期的cookie被称为会话cookie。会话cookie一般不保存在硬盘上而是保存在内存里.
+ 如果设置了过期时间，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览器，这些cookie依然有效直到超过设定的过期时间

### HttpSession

+ 在服务器端保持 HTTP 状态信息的方案
+ ***产生`Session`对象的过程***: 当程序需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否包含了一个session标识(即sessionId),如果已经包含一个sessionId则说明以前已经为此客户创建过session，服务器就按照session id把这个session检索出来使用(如果检索不到，可能会新建一个，这种情况可能出现在服务端已经删除了该用户对应的session对象，但用户人为地在请求的URL后面附加上一个JSESSION的参数)。如果客户请求不包含sessionId，则为此客户创建一个session并且生成一个与此session相关联的sessionId，这个session id将在本次响应中返回给客户端保存。
+ ***使用`Cookie`来跟踪Session***: session通过SessionID来区分不同的客户, session是以cookie或URL重写为基础的，默认使用cookie来实现，系统会创造一个名为JSESSIONID的输出cookie，这称之为session cookie,以区别persistent cookies(也就是我们通常所说的cookie),session cookie是存储于浏览器内存中的，并不是写到硬盘上的，通常看不到JSESSIONID，但是当把浏览器的cookie禁止后，web服务器会采用URL重写的方式传递

### HttpSession 的生命周期

+ ***创建一个HttpSession对象*** 一个常见的错误是以为session在有客户端访问时就被创建，然而事实是直到某server端程序(如Servlet)调用HttpServletRequest.getSession(true) 或者 HttpServletRequest.getSession()这样的语句时才会被创建
+ ***销毁HttpSession对象***
  1. 程序调用 HttpSession.invalidate()
  2. 距离上一次收到客户端发送的session id时间间隔超过了session的最大有效时间
  3. 服务器进程被停止(或当前Web应用被卸载) 注意：关闭浏览器只会使存储在客户端浏览器内存中的session cookie失效，不会使服务器端的session对象失效。

### HttpSession 相关的API

+ 获取`Session`对象 `request.getSession(), request.getSession(boolean create)`
+ 属性相关的: `setAttribute, getAttribute, removeAttribute`
+ 使Session失效：`invalidate()`
+ 设置其最大失效时间: `setMaxInactiveInterval`

### URL 重写

Servlet规范中引入了一种补充的会话管理机制，它允许不支持Cookie的浏览器也可以与WEB服务器保持连续的会话。
将会话标识号以参数形式附加在超链接的URL地址后面的技术称为URL重写。
代码:
```html
<a href="<%= response.encodeURL("login.jsp") %>"login</a>
```