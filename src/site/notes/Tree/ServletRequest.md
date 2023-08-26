---
{"dg-publish":true,"permalink":"/tree/servlet-request/","tags":["CS/programming-languages/java/javaweb/web-server/servlet"],"created":"2022-08-13T02:03:53.109+08:00","updated":"2023-08-27T03:51:47.639+08:00"}
---



Servlet程序通过ServletRequest封装获取到的HTTP请求数据

# Inheritance 

---

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/inheritance-of-ServletRequest.png)


---

1. web-server(如tomcat)需要解析请求数据，封装为ServletRequest对象，并且创建ServletRequest对象传递到service方法中
2. 使用该对象，查阅JavaEE API文档的HttpServletRequest接口

# Get Request Data

请求数据分为3部分：

1. 请求行：`GET/request-demo/req1?username=zhangsan HTTP/1.1`
	- String getMethod():获取请求方式：GET
	- String getContextPath():获取虚拟目录（项目访问路径）：/request-demo
	- StringBuffer getRequestURL():获取URL(统一资源定位符)：http://localhost:8080/request-demo/req
	- String getRequestURI():获取URI(统一资源标识符)：/request-demo/req1
	- String getQueryString():获取请求参数(GET方式)：username=zhangsan&password=123

2. 请求头：`User-Agent:Mozilla/5.0 Chrome/91.0.4472.106`
	- String getHeader(String name):根据请求头名称，获取值

3. 请求体：`username=superbaby&password=123`

	- ServletlnputStream getlnputStream():获取字节输入流
	- BufferedReader getReader():获取字符输入流

# Generic method to get the request parameters

请求参数获取方式：
- GET方式：
	- String getQueryString()
- POST方式
	- BufferedReader getReader()

---

- Map<String,String[]> getParameterMap():
	- 获取所有参数Map集合
- String[] getParameterValues(String name)：
	- 根据名称获取参数值（数组）
- String getParameter(String name):
	- 根据名称获取参数值（单个值）

---

- 使用通用方式获取请求参数后，屏蔽了GET和POST的请求方式代码的不同，则代码可以定义为如下格式：
```java
@WebServlet("/reqDemo3")
public class RequestDemo3 extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req,HttpServletResponse resp){}
	@Override
	protected void doPost(HttpServletRequest req,HttpServletResponse resp){
	this.doGet(req,resp);
	}
}
```

- 可以使用Servlet模板创建Servlet,更高效


# Solve Chinese Parameters Garbled

使用低版本的[[Tree/tomcat\|tomcat]]作为web-server，请求参数如果存在中文数据，则会乱码

解决方案：

- POST:设置输入流的编码
```java
req.setCharacterEncoding("UTF-8");
```

- 通用方式(GET/POST):先编码，再解码
```java
new String(username.getBytes("ISO-8859-1"),"UTF-8");
```

URL编码

1. 将字符串按照编码方式转为二进制
2. 每个字节转为2个16进制数并在前边加上%


Tomcat8.0之后，已将GET请求乱码问题解决，设
置默认的解码方式为UTF-8

# Forward

- 请求转发(forward): 一种在服务器内部的资源跳转方式

- 实现方式：

```java
req.getRequestDispatcher("资源B路径").forward(req,resp);
```


- 请求转发资源间共享数据：使用request对象
 
- void setAttribute(String name,Object o):存储数据到request域中
- Object getAttribute(String name):根据key,获取值
- void removeAttribute(String name):根据key,删除该键值对

请求转发特点：

- 浏览器地址栏路径不发生变化

- ==只能转发到当前服务器的内部资源==

- 一次请求，**可以在转发的资源间使用request共享数据**