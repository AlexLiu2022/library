---
{"dg-publish":true,"dg-path":"  microservices.md","permalink":"/microservices/","tags":["CS/architecture"],"created":"2022-09-08T02:15:29.746+08:00","updated":"2023-08-28T05:59:28.116+08:00"}
---


# Introduction

微服务

微服务是一种经过良好架构设计的[[Tree/architecture#distributed architecture\|分布式架构]]

微服务架构特征：
- 单一职责：微服务拆分粒度更小，每一个服务都对应唯一的业务能力，做到单一职责，避免重复业务开发
- 面向服务：微服务对外暴露业务接口
- 自治：团队独立、技术独立、数据独立、部署独立
- 隔离性强：服务调用做好隔离、容错、降级，避免出现级联问题


##  Structure  

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-microsevices-structure.png)

## Tech-Stack Comparison

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-microservices-tech-stack-comparison.png)

---

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-microservices-tech-stack-companies-may-use.png)


## SpringCloud

details see [[Tree/spring-cloud\|spring-cloud]]


# 服务拆分与远程调用

## 服务拆分注意事项

1. 不同微服务，不要重复开发相同业务
2. 微服务数据独立，不要访问其它微服务的数据库
3. 微服务可以将自己的业务暴露为接口，供其它微服务调用

## 微服务调用方式

- 基于RestTemplate,发起的[[Tree/HTTP\|http]]请求实现远程调用
- http请求做远程调用是与语言无关的调用，只要知道对方
的ip、端口、接口路径、请求参数即可。

## 提供者与消费者
- 服务提供者：一次业务中，被其它微服务调用的服务。（提供接口给其它微服务)
- 服务消费者：一次业务中，调用其它微服务的服务。（调用其它微服务提供的接口)


## Demo  

**根据订单 id 查询订单功能**

需求：根据订单id查询订单的同时，把订单所属的用户信息一起返回

1. 注册RestTemplate

在order-service的OrderApplication中注册RestTemplate

```java
@MapperScan("cn.test.order.mapper")
@SpringBootApplication
public class OrderApplication
public static void main(String[]args){
SpringApplication.run(OrderApplication.class,args);
}
③Bean
public RestTemplaterestTemplate(){
return new RestTemplate();
```

2. 服务远程调用RestTemplate

修改order--service中的OrderService的queryOrderByld方法：

```java
@Service
public class OrderService
@Autowired
private RestTemplate restTemplate;
public Order queryOrderById(Long orderId){
//1.查询订单
Orderorder orderMapper.findById(orderId);
//T0D02.查询用户
String url "http://Localhost:8081/user/"+order.getUserId();
Useruser restTemplate.getForObject(url,User.class);
//3.封装User信息
order.setUser(user);
//4.返回
return order;
}
```

**存在的问题**

- 服务消费者该如何获取服务提供者的地址信息？
- 如果有多个服务提供者，消费者该如何选择？
- 消费者如何得知服务提供者的健康状态？

使用注册中心中间件[[Tree/eureka\|eureka]]解决

# Tech Map

- [[Tree/middlewares\|middlewares]]
