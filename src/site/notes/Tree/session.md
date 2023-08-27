---
{"dg-publish":true,"dg-path":"  session.md","permalink":"/session/","tags":["CS/web","CS/programming-languages/java/javaweb"],"created":"2022-08-14T01:08:44.995+08:00","updated":"2023-08-27T03:11:58.293+08:00"}
---


# Introduction

**Defination:**

In computer science and networking in particular, a session is a time-delimited two-way link, a practical (relatively high) layer in the tcp/ip protocol enabling interactive expression and information exchange between two or more communication devices or ends – be they computers, automated systems, or live active users (see login session). A session is established at a certain point in time, and then ‘torn down’ - brought to an end - at some later point. An established communication session may involve more than one message in each direction. A session is typically stateful, meaning that at least one of the communicating parties needs to hold current state information and save information about the session history to be able to communicate, as opposed to stateless communication, where the communication consists of independent requests with responses.

---

会话：用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束.在一次会话中可以包含多次请求和响应.

---


# Session Tracking

Session Tracking tracks a user’s requests and maintains their state.

Details see [[Tree/session-tracking\|session-tracking]]