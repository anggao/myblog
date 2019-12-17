---
title: "JavaWeb Filter"
date: "2014-11-16"
template: "post"
draft: false
slug: "javaweb-filter"
category: "java"
tags:
  - "java"
description: ""
socialImage: ""
---

### Filter 是什么

`JavaWeb` 的一个重要组件，可以对发送到 Servlet 的请求进行拦截，并对响应也可以进行拦截 Filter 是实现了 Filter 接口的 Java 类 Filter 需要在 web.xml 文件中进行配置和映射

### 如何创建 Filter

```xml
<!-- 第一步 -->
public class HelloFilter implements Filter

<!--  注册Filter -->
<filter>
    <filter-name>helloFilter</filter-name>
    <filter-class>com.ang.javaweb.HelloFilter</filter-class>
    <init-param>
        <param-name>name</param-name>
        <param-value>root</param-value>
    </init-param>
</filter>

<!-- 映射Filter -->
<filter-mapping>
    <filter-name>helloFilter</filter-name>
    <url-pattern>/test.jsp </url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

### Filter 相关的 API

#### Filter 接口

- `public void init(FilterConfig filterConfig)`: 类似与 Servlet 的 init 方法，在创建 Filter 对象后(Filter 对象在 Servlet 容器加载当前 Web 应用时即被创建)，立即被调用,且只被调用一次. Filter 实例是单例的.
  - FilterConfig 类似于 ServletConfig
  - 可以在 web.xml 中配置当前 Filter 的初始化参数.
- `public void doFilter(ServletRequest request, ServletResponse response,FilterChain chain)` 真正 Filter 的逻辑代码,
  - `FilterChain`: Filter 链，多个 Filter 可以构成一个 Filter chain.
    - `doFilter(ServletRequest request, ServletResponse response)`: 把请求传给 Filter chain 的下一个 Filter, 若当前 Filter 是 Filter chain 的最后一个 Filter, 将把请求给到目标 Servlet (JSP).
    - 多个 Filter 拦截的顺序和`<filter-mapping>`配置的顺序有关，靠前的先被调用.
- `public void destroy()`: 在 Web 容器卸载 Filter 对象之前被调用。该方法在 Filter 的生命周期中仅执行一次

### <dispatcher> 元素指定过滤器所拦截的资源被 Servlet 容器调用的方式

- 可以是`REQUEST,INCLUDE,FORWARD`和`ERROR`之一，默认`REQUEST`.
- 可以设置多个`<dispatcher>` 子元素用来指定 `Filter` 对资源的多种调用方式进行拦截
  - `REQUEST`：当用户直接访问页面时，Web 容器将会调用过滤器。如果目标资源是通过 RequestDispatcher 的 include()或 forward()方法访问时，那么该过滤器就不会被调用。
  - `INCLUDE`：如果目标资源是通过 RequestDispatcher 的 include()方法访问时，那么该过滤器将被调用。除此之外，该过滤器不会被调用。
    - `<jsp:include file="/.."/>`
  - `FORWARD`：如果目标资源是通过 RequestDispatcher 的 forward()方法访问时，那么该过滤器将被调用，除此之外，该过滤器不会被调用。
    - `<jsp:forward page="" />` 或通过 page 指令的 errorPage 转发页面<%@ page errorPage="error.jsp" %>。
  - `ERROR`：如果目标资源是通过声明式异常处理机制调用时，那么该过滤器将被调用。除此之外，过滤器不会被调用
    - 在`web.xml` 中

```xml
<error-page>
    <exception-type>java.lang.ArithmeticException</exception-type>
    <location>/test.jsp</location>
</error-page>
```
