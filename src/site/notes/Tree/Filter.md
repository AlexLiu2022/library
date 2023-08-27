---
{"dg-publish":true,"dg-path":"  Filter.md","permalink":"/filter/","tags":["CS/web","CS/programming-languages/java/javaweb"],"created":"2022-08-14T14:49:33.405+08:00","updated":"2023-08-27T03:10:09.882+08:00"}
---


# Introduction

- 概念：Filter表示过滤器，是JavaWeb三大组件(Servlet Filter Listener)之一
- 过滤器可以把对资源的请求==拦截==下来，从而实现一些特殊的功能
- 过滤器一般完成一些==通用==的操作，比如：权限控制、统一编码处理、敏感字符处理等等…

# Get Started

1. 定义类，实现Filter接口，并重写其所有方法
```java
public class FilterDemo implements Filter{
	public void init(FilterConfig filterconfig)
	public void doFilter(ServletRequest request
	public void destroy(){
}
```
2. 配置Filter拦截资源的路径：在类上定义@WebFilter注解
```java
@WebFilter("/*")
public class FilterDemo implements Filter
```
3. 在doFilter,方法中输出一句话，并放行
```java
public void doFilter(ServletRequest request,Ser
	System.out.println("filter被执行了..")；
	//放行
	chain.doFilter(request,response);
}										 
```



# Process

1. 执行放行前逻辑
2. 放行
3. 访问资源
4. 执行放行后逻辑

# Details

Filter可以根据需求，配置不同的拦载资源路径
```java
@WebFilter("/*")
public class FilterDemo
```

拦截具体的资源：`/index.jsp`:只有访问index,jsp时才会被拦截。
目录拦截：`/user/*`:访问/user下的所有资源，都会被拦截
后缀名拦截：`*.jsp`:访问后缀名为jsp的资源，都会被拦截
拦截所有：`/*`：访问所有资源，都会被拦截

**过滤器链**

一个Web应用，可以配置多个过滤器，这多个过滤器称为过滤器链

注解配置的Filter,优先级按照过滤器类名（字符串）的自然排序