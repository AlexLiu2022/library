---
{"dg-publish":true,"permalink":"/tree/tomcat/","tags":["CS/web/web-server ","CS/programming-languages/java/javaweb/web-server"],"created":"2022-08-11T14:11:57.055+08:00","updated":"2023-08-27T03:20:33.999+08:00"}
---


# Introduction

Defination:

Apache Tomcat is a free and open-source implementation of the Jakarta Servlet, Jakarta Expression Language, and WebSocket technologies. Tomcat provides a "pure Java" HTTP web server environment in which Java code can run.

---

==Apache Tomcat is the servlet container as well as a web server==

---

# 配置和部署项目

- 配置：

1. 修改启动端口号：conf/server.xml

```xml
<Connector
port="8080"protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8443"/>
```
- 注：HTTP协议默认端口号为80，如果将Tomcat端口号改为80，则将来访问Tomcat时，将不用输入端口号

---

- 启动时可能出现的问题：
1. 端口号冲突：找到对应程序，将其关闭掉
```shell
at org.apache.catalina.startup.Bootstrap.load (Bootstrap.java
at org.apache.catalina.startup.Bootstrap.rain(Bootstrap.java
Caused by:java.net.BindException:Address already in use:bind
at sun.nio.ch.Net.bind0 (Native Method)
at sun.nio.ch.Net.bind (Net.java:433)
```

2. 启动窗口一闪而过：检查JAVA_HOME环境变量是否正确配置

- 部署项目：

	- 将项目放置到webapps目录下，即部署完成

- 一般JavaWeb项目会被打成war包，然后将war包放到webapps目录下，Tomcat会自动解压缩war文件


# IDEA集成本地Tomcat

IDEA中使用Tomcat： 
- 集成本地Tomcat
	- 将本地Tomcat集成到ldea中，然后进行项目部署即可
- Tomcat Maven插件
	1. pom.Xml添加Tomcat插件
	2. 使用Maven Helper插件快速启动项目，选中项目，右键->Run Maven->tomcat7:run
