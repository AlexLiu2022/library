---
{"dg-publish":true,"dg-path":"  Servlet.md","permalink":"/servlet/","tags":["CS/web/web-server","CS/programming-languages/java/javaweb/web-server"],"created":"2022-08-11T14:23:46.846+08:00","updated":"2023-08-27T03:07:52.231+08:00"}
---


# Introdution

Defination:

A Jakarta Servlet (formerly Java Servlet) is a Java software component that extends the capabilities of a server. Although servlets can respond to many types of requests, they most commonly implement web containers for hosting web applications on web servers and thus qualify as a server-side servlet web API. Such web servlets are the Java counterpart to other dynamic web content technologies such as PHP and ASP.NET.

---

Servlet是Java提供的一门动态web资源开发技术

Servlet是JavaEE规范之一，是一个接口，需要定义Servlet类实现Servlet接口，并由web服务器（如[[Tree/tomcat\|tomcat]])运行Servlet.


# Get Started

1. 创建web项目，导入Servlet依赖坐标

```xml
<dependency>
<groupld>javax.servlet</groupld>
<artifactld>javax.servlet-api</artifactld>
<version>3.1.0</version>
<scope>provided</scope>
</dependency>
```
2. 创建：定义一个类，实现Servlet接口，并重写接口中所有方法，并在service方法中输入一句话
```java
public class ServletDemo1 implements Servlet
public void service()
```
3. 配置：在类上使用@WebServlet注解，配置该Servlet的访问路径
```java
@WebServlet("/demo1")
public class ServletDemo1 implements Servlet
```
4. 访问：启动Tomcat,浏览器输入URL访问该Servlet

`http://localhost:8080/web-demo/demo1`

# Process & Lifecircle

执行流程

- Servlet由web服务器创建，Servlet方法由web服务器调用。
- 因为自定义的Servlet必须实现Servlet接口并覆写其方法，而Servlet接口中有service方法，所以web服务器知道servlet中一定有service方法

 生命周期
 
- 对象的生命周期指一个对象从被创建到被销毁的整个过程
- Servlet运行在Servlet容器(web服务器)中，其生命周期由容器来管理，分为4个阶段：
1. 加载和实例化：**默认情况**下，当Servlet**第一次被访问时**，由容器创建Servlet对象
	1. 负整数：第一次被访问时创建Servlet)对象
	2. 0或正整数：**服务器启动时**创建Servlet对象,==数字越小优先级越高==

```java
@WebServlet(urlPatterns "/demo",
loadOnStartup =1)
```

2. 初始化：在Servlet实例化之后，容器将调用Servlet的init()方法初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作。==该方法只调用一次==
3. 请求处理：每次请求Servlet时，Servlet容器都会调用Servlet的service()方法对请求进行处理。
4. 服务终止：当需要释放内存或者容器关闭时，容器就会调用Servlet实例的destroy()方法完成资源的释放。在destroy()方法调用之后，容器会释放这个Servlet?实例，该实例随后会被Java的垃圾收集器所回收

# Methods & Architecture

**Methods:**

- 初始化方法，在Servlet被创建时执行，只执行一次
```java
void init(ServletConfig config)
```
- 提供服务方法，每次Servlet被访问，都会调用该方法
 ```java
void service(ServletRequest req,ServletResponse res)
```
- 销毁方法，当Servlet被销毁时，调用该方法。在内存释放或服务器关闭时销毁Servlet
```java
void destroy()
```
- 获取ServletConfig对象
```java
ServletConfig getServletConfig()
```
- 获取Servlet信息
```java
String getServletInfo()
```

---

**Architecture**

---

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/architecture-of-servlet.png)

---

开发B/S架构的web项目，都针对HTTP协议,所以自定义Servlet,会继承HttpServlet

```java
@Webservlet("/demo4")
public class ServletDemo4 extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req,HttpServletResponse resp)
		//T0D06et清求方式处理逻辑
}
	@Override
	protected void doPost(HttpServletRequest req,HttpServletResponse resp)
		//T0D0Post请求方式处理逻辑
}
```

1. HttpServlet使用步骤
	1. 继承HttpServlet
	2. 重写doGet和doPost方法
2. HttpServlet原理
	- 获取请求方式，并根据不同的请求方式，调用不同的doXxx方法

# Config urlPattern 

Servlet要想被访问，必须配置其访问路径(urlPattern)

1. 一个Servlet,可以配置多个urlPattern
```java
@Webservlet(urlPatterns {"/demo1","/demo2"})
```
2. urlPattern配置规则
	1. 精确匹配
		1. 配置路径：`@WebServlet("/user/select")`
		2. 访问路径：`localhost8080/web-demo/user/select`

	3. 目录匹配
		1. 配置路径：`@WebServlet("/User/*")`
		2. 访问路径：
			1.`localhost:8080/web-demo/user/bbb`
			2.`localhost:8080/web-demd/user/aaa`
	4. 扩展名匹配
		1. 配置路径：`@WebServlet("\*.do")`
		2. 访问路径：
			1. `localhost:8080/web-demo/bbb.do`
			2. `localhost:8080/web-demd/aaa.do`
	5. 任意匹配
		1. 配置路径
			1. `@WebServlet("/*")`
			2. `@Webservlet("/")`
		2. 访问路径：
			1. `localhost:8080/web-demd/hehe`
			2. `localhost:8080/web-demo/haha`
		3. /和/\*区别：
			1. 当项目中的Servlet配置了"/"，会覆盖掉tomcat中的DefaultServlet,当其他的url-pattern都匹配不上时都会走这个Servlet
			2. 当项目中配置了“/\*”，意味着匹配任意访问路径

	6. 优先级：精确路径>目录路径>扩展名路径>/\*>/

# config in XML way

**XML配置方式编写Servlet**

Servlet从3.0版本后开始支持使用注解配置，3.0版本前只支持XML配置文件的配置方式

步骤：
1. 编写Servlet类
2. 在web.xml中配置该Servlet
```xml
<servlet>
	<servlet-name>demo5</servlet-name>
	<servlet-class>com.servlet.web.servlet.ServletDemo5</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>demo5</servlet-name>
	<url-pattern>/demo5</url-pattern>
</servlet-mapping>
```

# Req  & Res in servlet 

- [[Tree/ServletRequest\|ServletRequest]]
- [[Tree/ServletResponse\|ServletResponse]]

**路径问题**

- 浏览器使用：需要加虚拟目录（项目访问路径）

- 服务端使用：不需要加虚拟目录

动态获得虚拟目录:

- 用`String getContextPath()`方法的返回值配置路径