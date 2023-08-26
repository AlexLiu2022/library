---
{"dg-publish":true,"permalink":"/tree/connection-pooling/","tags":["CS/database"],"created":"2022-08-09T03:35:22.182+08:00","updated":"2023-08-27T04:15:40.434+08:00"}
---


# Introduction

数据库连接池是个容器，负责分配、管理数据库连接(Connection)

它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；
释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

好处：
- 资源重用
- 提升系统响应速度
- 避免数据库连接遗漏

# Implement

数据库连接池实现:
- 标准接口：DataSource
	- 官方(SUN)提供的数据库连接池标准接口，由第三方组织实现此接口。
	- 功能：获取连接 Connection getConnection()

- 常见的数据库连接池：
	- DBCP
	- C3P0
	- Druid


# Java connection pooling
- [[Tree/Druid\|Druid]]