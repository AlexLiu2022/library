---
{"dg-publish":true,"dg-path":"  spring-security.md","permalink":"/spring-security/","tags":["CS/programming-languages/java/java-frameworks/spring"],"created":"2023-06-05T18:30:00.214+08:00","updated":"2023-09-14T17:17:26.324+08:00"}
---


## 介绍

一般来说，Web 应用的安全性包括**用户认证（Authentication）和用户授权（Authorization）**两个部分，这两点也是 SpringSecurity 重要核心功能。

（1）用户认证指的是：验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码，系统通过校验用户名和密码来完成认证过程。

**通俗点说就是系统认为用户是否能登录**

（2）用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

**通俗点讲就是系统判断用户是否有权限去做某些事情。**

## 实现

Spring Security进行认证和鉴权的时候,是利用的一系列的[[Tree/Filter\|Filter]]来进行拦截的。

![diagram-of-spring-security-impl-filter-chain.png](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-spring-security-impl-filter-chain.png)


如图所示，一个请求想要访问到API就会从左到右经过蓝线框里的过滤器，其中**绿色部分是负责认证的过滤器，蓝色部分是负责异常处理，橙色部分则是负责授权**。进过一系列拦截最终访问到我们的API。

1. `FilterSecurityInterceptor`:是一个方法级的权限过滤器，基本位于过滤链的最底部。
2. `ExceptionTranslationFilter`:是个异常过滤器，用来处理在认证授权过程中抛出的异常。
3. `UsernamePasswordAuthenticationFilter`:对/login的POST请求做拦截，校验表单中用户

这里面只需要重点关注两个过滤器即可：`UsernamePasswordAuthenticationFilter`负责登录认证，`FilterSecurityInterceptor`负责权限授权。

说明：**Spring Security的核心逻辑全在这一套过滤器中，过滤器里会调用各种组件完成功能，掌握了过滤器和组件就掌握了Spring Security**！框架的使用方式就是对这些过滤器和组件进行扩展。

## 入门

认证实现

1. 输入用户名和密码，提交
2. 把提交用户名和密码封装对象
34. 调用方法实现认证
5. 调用方法，根据用户名查询用户信息
6. 查询用户信息返回对象
7. 密码比较
8. 填充回，返回
9. 返回对象放到上下文对象里面


授权