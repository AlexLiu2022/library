---
{"dg-publish":true,"dg-path":"  Cookie.md","permalink":"/cookie/","tags":["CS/web/session/session-tracking","CS/programming-languages/java/javaweb/session/session-tracking"],"created":"2022-08-14T01:46:37.822+08:00","updated":"2023-08-27T03:08:52.973+08:00"}
---


Cookie:客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问 

# Basic Usage

1. 创建Cookie对象，设置数据
```java
Cookie cookie new Cookie("key","value");
```
2. 发送Cookie到客户端：使用response对象
```java 
response.addCookie(cookie);
```
---

3. 获取客户端携带的所有Cookie,使用request对象
```java
Cookie[]cookies request.getCookies();
```
4. 遍历数组，获取每一个Cookie对象：for
5. 使用Cookie对象方法获取数据
```java
cookie.getName();
cookie.getValue();
```

# Principle

**Cookie原理**

Cookie的实现是基于HTTP协议的
- 响应头：set-cookie
- 请求头：cookie

# Details

 ## Cookie存活时间
 
 - 默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁
 
- setMaxAge(int seconds):设置Cookie存活时间
	1. 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除
	2. 负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie被销毁
	3. 零：删除对应Cookie

## Cookie存储中文

Cookie不能直接存储中文

如需要存储，则需要进行转码：URL编码