---
{"dg-publish":true,"dg-path":"  java-web.md","permalink":"/java-web/","tags":["CS/programming-languages/java","CS/web"],"created":"2022-08-02T16:55:18.126+08:00","updated":"2023-08-27T03:06:58.340+08:00"}
---


Java-Web 技术栈

- B/S架构：
	- Browser/Server,浏览器/服务器架构模式，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可
	- 好处：
		- 易于维护升级：服务器端升级后，客户端无需任何部署就可以使用到新的版本 


---

- 静态资源：HTML、CSS、JavaScript、图片等 负责页面展现
- 动态资源：[[Tree/Servlet\|Servlet]], JSP等 ,负责逻辑处理
- 数据库：负责存储数据
- [[Tree/HTTP\|HTTP]]协议：定义通信规则
- Web服务器[[Tree/web-server\|web-server]]：负责解析HTTP协议，解析请求数据，并发送响应数据
- 服务器与客户端每一次建立连接到断开连接，称作一次会话（[[Tree/session\|session]]）


---

java-web 三大组件

- [[Tree/Servlet\|Servlet]]
- [[Tree/Filter\|Filter]]
- [[Tree/Listener\|Listener]]

---

Web-framework

- [[Tree/spring-MVC\|spring-MVC]]