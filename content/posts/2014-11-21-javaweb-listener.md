---
title: "JavaWeb Listener"
date: "2014-11-21"
template: "post"
draft: false
slug: "javaweb-listener"
category: "java"
tags:
  - "java"
description: ""
socialImage: ""
---

### Servlet 监听器

`Servlet`监听器：`Servlet` 规范中定义的一种特殊类，它用于监听 `web` 应用程序中的 `ServletContext`, `HttpSession` 和 `ServletRequest` 等域对象的创建与销毁事件，以及监听这些域对象中的属性发生修改的事件

### Servlet 监听器的分类

- 监听域对象自身的创建和销毁的事件监听器
  - `ServletContextListener`
  - `HttpSessionListener`
    - 在线用户统计，可以知道是否有用户访问当前的 Web 应用 (大致数据，无法探知用户关闭浏览器)
  - `ServletRequestListener`
- 监听域对象中的属性的增加和删除的事件监听器
  - `ServletContextAttributeListener`
  - `HttpSessionAttributeListener`
  - `ServletRequestAttributeListener`
- 监听绑定到 `HttpSession` 域中的某个对象的状态的事件监听器
  - HttpSessionBindingListener
  - HttpSessionActivationListener
    - Java 类需要实现 `Serializable` 接口, 否则只能写到磁盘，不能读取
    - `session` 对象存储在`work\Catalina\localhost\contextPath` 目录下, `SESSION.SER`
  - 实现这两个接口的类不需要 web.xml 文件中进行注册

### 编写 Servlet 监听器

- 编写实现监听器接口的`Java`类
- 对于第一种和第二种监听器需要在`web.xml`中注册
- 一个 `web.xml` 文件中可以注册多个 `Servlet` 事件监听器，`web` 服务器按照它们在 `web.xml` 文件中的注册顺序来加载和注册这些 `Serlvet` 事件监听器
- 由于一个 `web` 应用程序只会为每个事件监听器创建一个对象，有可能出现多个线程同时调用同一个事件监听器对象的情况，所以，在编写事件监听器类时，应考虑多线程安全的问题

### ServletContextListener

- 监听`ServletContext`对象被创建或销毁的`Servlet`监听器
- 可以在当前 Web 应用被加载时对当前应用的相关资源进行初始化操作:
  - 创建数据库连接池
  - 创建 Spring 的 IOC 容器
  - 读取当前 Web 应用的初始化参数
- `public class HelloServletContextListener implements ServletContextListener`
- 在 web.xml 中配置 Listener

```xml
<listener>
    <listener-class>com.ang.test.HelloServletContextListener</listener-class>
</listener>
```

- API
  - `public void contextInitialized(ServletContextEvent sce)`
  - `public void contextDestroyed(ServletContextEvent sce)`
- 其他 Listener
  - HttpSessionListener
  - ServletRequestListener
