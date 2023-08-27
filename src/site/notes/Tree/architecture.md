---
{"dg-publish":true,"dg-path":"  architecture.md","permalink":"/architecture/","tags":["CS"],"created":"2022-10-03T16:24:06.996+08:00","updated":"2023-08-28T05:04:20.362+08:00"}
---


**Architecture** 

架构

# monolithic architecture

单体架构：

将业务的所有功能集中在一个项目中开发，打成一个包部署。

优点：
- 架构简单
- 部署成本低

缺点：
- 耦合度高
# distributed architecture
分布式架构：
根据业务功能对系统进行拆分，每个业务模块作为独立项目开发，称为一个服务。

优点：
- 降低服务耦合
- 有利于服务升级拓展

分布式架构的要考虑的问题：

- 服务拆分粒度如何？
- 服务集群地址如何维护？
- 服务之间如何实现远程调用？
- 服务健康状态如何感知？

## microservices

details see [[Tree/microservices\|microservices]]