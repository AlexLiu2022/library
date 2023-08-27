---
{"dg-publish":true,"dg-path":"  session-tracking.md","permalink":"/session-tracking/","tags":["#CS/web/session","#CS/programming-languages/java/javaweb/session"],"created":"2022-08-14T01:14:57.728+08:00","updated":"2023-08-27T03:14:48.387+08:00"}
---



会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间==共享数据==

 [[Tree/HTTP\|HTTP]]协议是无状态的，每次浏览器向服务器请求时，服务器都会将该请求视为新的请求，因此需要会话跟踪技术来实现会话内数据共享

实现方式：

1. 客户端会话跟踪技术：[[Tree/Cookie\|Cookie]]
2. 服务端会话跟踪技术：[[Tree/HttpSession\|HttpSession]] 


---

- Cookie和Session都是来完成一次会话内多次请求间**数据共享** 的

区别：

- 存储位置：Cookie是将数据存储在客户端，Session将数据存储在服务端
- 安全性：Cookie不安全，Session安全
- 数据大小：Cookie最大3KB,Session无大小限制
- 存储时间：Cookie可以长期存储，Session默认30分钟
- 服务器性能：Cookie不占服务器资源，Session占用服务器资源