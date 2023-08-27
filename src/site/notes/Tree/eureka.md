---
{"dg-publish":true,"dg-path":"  eureka.md","permalink":"/eureka/","tags":["CS/microservices/middlewares"],"created":"2022-11-04T17:48:28.275+08:00","updated":"2023-08-27T05:13:02.496+08:00"}
---


# Introduction


在Eureka架构中，微服务角色有两类：

- EurekaServer:服务端，注册中心
	- 记录服务信息
	- 心跳监控
- EurekaClient:客户端
	- Provider:服务提供者，例如案例中的user-service
		- 注册自己的信息到EurekaServer
		- 每隔30秒向EurekaServer发送心跳
	- consumer:服务消费者，例如案例中的order-service
		- 根据服务名称从EurekaServer拉取服务列表
		- 基于服务列表做负载均衡，选中一个微服务后发起远程调用


![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-how-eureka-works.png)



# Build

## 搭建EurekaServer

搭建EurekaServer服务步骤如下：

1. 创建项目，引入spring-cloud-starter-netflix-eureka-server的依赖

```xml
<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```
2. 编写启动类，添加`@EnableEurekaServer`注解 
3. 添加application.yml文件，编写下面的配置
```yml
server:
	port: 10086
spring:
	application:
		name: eurekaserver
eureka:
	client:
		service-url:
			defaultZone: http://127.0.0.1:10086/eureka/
```