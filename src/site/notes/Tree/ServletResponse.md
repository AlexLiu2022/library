---
{"dg-publish":true,"dg-path":"  ServletResponse.md","permalink":"/servlet-response/","tags":["CS/programming-languages/java/javaweb/web-server/servlet"],"created":"2022-08-13T20:48:49.157+08:00","updated":"2023-08-27T03:52:14.552+08:00"}
---


 # Response设置响应数据
 
响应数据分为3部分：
1. 响应行： HTTP/1.1 200 OK
- void setStatus(int sc):设置响应状态码
2. 响应头：Content-Type:text/html
- void setHeader(String name,String value):设置响应头键值对
3. 响应体：
```html
<html><head>head><body></body></html>
```
- PrintWriter getWriter():获取字符输出流
- ServletOutputStream getOutputStream():获取字节输出流

# Response完成重定向

重定向(Redirect):一种资源跳转方式
  
实现方式：

```java
resp.setStatus(302);
resp.setHeader("location”,“资源B的路径")；
``` 
简化实现方式：
```java
resp.sendRedirect("资源B的路径")；
``` 

重定向特点：

- 浏览器地址栏路径发生变化

- 可以重定向到任意位置的资源（服务器内部、外部均可）

- 两次请求，**不能在多个资源使用request共享数据**

# Response响应字符数据

**使用**

1. 通过Response对象获取字符输出流
```java
PrintWriter writer = resp.getWriter();
```
2. 写数据
```java
writer.write("aaa");
```  

注意：
- 该流不需要关闭，随着s响应结束，response对象销毁，由服务器关闭
- 中文数据乱码：原因通过Response:获取的字符输出流默认编码：ISO-8859-1
```java
resp.setContentType("text/html;charset=utf-8");// 设置类型否则不会解析HTML标签
```


# Response响应字节数据

使用：
1. 通过Response对象获取字符输出流
```java
ServletOutputStream outputStream =  resp.getOutputStream();
```
2. 写数据
```java
outputStream.write(字节数据);
```

IOUtils工具类使用

1. 导入坐标
```xml
<dependency>
<groupld>commons-io</groupld>
<artifactld>commons-io</artifactld>
<version>2.6</version>
</dependency>
```
2. 使用
```java
IOUtils.copy(输入流，输出流)；
```