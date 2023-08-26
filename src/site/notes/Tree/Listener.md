---
{"dg-publish":true,"permalink":"/tree/listener/","tags":["CS/web","CS/programming-languages/java/javaweb"],"created":"2022-08-14T16:04:09.204+08:00","updated":"2023-08-27T03:11:49.095+08:00"}
---


# Introduction

- 概念：Listener表示监听器，是JavaWeb三大组件(Servlet Filter Listener)之一
- 监听器是可以监听application,session,request三个对象,在其创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件
- Listener分类：JavaWeb中提供了8个监听器

---

| 监听器分类         | 监听器名称                      | 作用                                            |
| ------------------ | ------------------------------- | ----------------------------------------------- |
| ServletContext监听 | ServletContextListener          | 用于对ServletContext对象进行监听（创建、销毁）  |
|                    | ServletContextAttributeListener | 对ServletContexti对象中属性的监听（增删改属性） |
| Session监听        | HttpSessionListener             | 对Session对象的整体状态的监听（创建、销毁）     |
|                    | HttpSessionAttributeListener    | 对Session对象中的属性监听（增删改属性）         |
|                    | HttpSessionBindingListener      | 监听对象于Session的绑定和解除                   |
|                    | HttpSessionActivationListener   | 对Session数据的钝化和活化的监听                 |
| Request监听        | ServletRequestListener          | 对Request对象进行监听（创建、销毁）            |
|                    | ServletRequestAttributeListener |                                             对Request对象中属性的监听（增删改属性）|

# How to use

ServletContextListener使用

1. 定义类，实现ServletContextListener接口
```java
public class ContextLoaderListener implements ServletContextListener{ 
/**
*ServletContext对象被创建：整个web应用发布成功
sce
*/
public void contextInitialized(ServletContextEvent sce){}
/* 
*ServletContext对象被销毁：整个web应用卸载
sce
*/八
public void contextDestroyed(ServletContextEvent sce){}
}
```

