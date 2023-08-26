---
{"dg-publish":true,"permalink":"/tree/database/","created":"2022-07-31T14:24:13.599+08:00","updated":"2023-08-26T19:55:09.825+08:00"}
---


## Defination 

A database is an organized collection of structured information, or data, typically stored electronically in a computer system. ==A database is usually controlled by a database management system (DBMS).== Together, the data and the DBMS, along with the applications that are associated with them, are referred to as a database system, often shortened to just database.

Data within the most common types of databases in operation today is typically modeled in rows and columns in a series of tables to make processing and data querying efficient. The data can then be easily accessed, managed, modified, updated, controlled, and organized. Most databases use ==structured query language==    ([[Tree/SQL\|SQL]]) for writing and querying data.

---

## Common DBMS

(DataBase Management System)

 - [[Tree/mysql\|mysql]]
 - [[oracle\|oracle]]
 - ...

---

 ## Design Paradigm


**三大范式（即数据库表的设计依据  ）**

1.  要求任何一张表必须有主键，每个字段原子性不可再分
2. 建立在第一范式基础上，要求所有非主键字段完全依赖主键，不要产生部分依赖
3. 建立在第二范式基础上，要求所有非主键字段直接依赖主键，不要产生传递依赖

设计数据库表的时遵循以上范式 ，可避免表中数据冗余  空间浪费


---

### NO.1

最核心，最重要的范式，所有表的设计都需要满足。

必须有主键，并且每一个字段都是原子性不可再分。

---
观察下表：

|学生编号 | 学生姓名 | 联系方式                 |
| -------- | -------- | ------------------------ |
| 1001     | 张三     | zs@gmail.com,1359999999  |
| 1002     | 李四     | ls@gmai1.com,13699999999 |
| 1001     | 王五     | ww@163.net,13488888888  |


---
该表不符合第一范式，因为：

1. 学生编号重复，没有主键

2. 联系方式字段不具备有原子性

---

正确的写法：

| 学生编号（PK) | 学生姓名 | 联系邮箱     | 联系电话    |
| ------------- | -------- | ------------ | ----------- |
| 1001          | 张三     | zs@gmail.com | 1359999999  |
| 1002          | 李四     | ls@gmai1.com | 13699999999 |
| 1003          | 王五     | ww@163.net   | 13488888888 | 



---

### NO.2

| 学生编号+教师编号 （PK） | 学生姓名 | 教师姓名 |
| ------------------------ | -------- | -------- |
| 1001        001          | 张三     | 王老师   |
| 1002        002          | 李四     | 赵老师   |
| 1003        001          | 王五     | 王老师   |
| 1001        002          | 张三     | 赵老师   | 

观察上表

---
主键： 学生编号+教师编号，两个字段联合做复合主键,表满足了第一范式 

但不满足第二范式，"张三"依赖1001，"王老师"依赖001，产生了部分依赖

产生部分依赖的缺点：

数据冗余，空间浪费  ,"张三"重复了，"王老师"重复了

为了使以上表满足第二范式——使用三张表表示多对多的关系

---

- 学生表 

| 学生编号（PK） | 学生姓名 |
| -------------- | -------- |
| 1001           | 张三     |
| 1002           | 李四     |
| 1003           | 王五     |

- 教师表

| 教师编号(PK) | 教师姓名 |
| ------------ | -------- |
| 001          | 王老师   |
| 002          | 赵老师   |

---

- 学生教师关系表

| id(Pk) | 学生编号（FK） | 教师编号（FK） |
| ------ | -------------- | -------------- |
| 1      | 1001           | 001            | 
| 2      | 1002           | 002            |
| 3      | 1003           | 001            |
| 4      | 1001           | 002            |

---
多对多设计：==多对多，三张表，关系表两个外键==

---

### NO.3

| 学生编号(PK) | 学生姓名 | 班级编号 | 班级名称 |
| ------------ | -------- | -------- | -------- |
| 1001         | 张三     | 01       | 一年一班 |
| 1002         | 李四     | 02       | 一年二班 |
| 1003         | 王五     | 03       | 一年三班 |
| 1004         | 赵六     | 03       | 一年三班 | 

 分析以上的表：
 
 一年一班依赖01,而01依赖1001，产生了传递依赖 这不符合第三范式的要求 造成数据冗余

---
正确设计一对多：

班级表（一）

| 班级编号 | 班级名称 |
| -------- | -------- |
| 01       | 一年一班 |
| 02       | 一年二班 | 
| 03       | 一年三班 |
| 03       | 一年三班 |

---
学生表（多）

| 学生编号(PK) | 学生姓名 | 班级编号（PK） | 
| ------------ | -------- | -------------- |
| 1001         | 张三     | 01             |
| 1002         | 李四     | 02             |
| 1003         | 王五     | 03             |
| 1004         | 赵六     | 03             |

==一对多，两张表，多的表加外键==

---

在实际的开发中，可能存在一张表字段太多，太庞大，这个时候也要拆分表

---
没有拆分表之前：一张t_user表:

| id  | login_name | login_pwd | real_name | email_address | ... |     |     |
| --- | ---------- | --------- | --------- | ------------- | --- | --- | --- |
| 1   | sansan     | 12345     | zhangsan  | zs@qq.com     | ... |     |     |

---

这种庞大的表建议拆分为两张：
1. t_1ogin: 登录信息表

2. t_user: 用户详细信息表

---

登录信息表t_login

| id(pk) | login_name | login_pwd |
| ------ | ---------- | --------- |
| 1      | zhangsan   | 123       | 
| 2      | lisi       | 123       |


---
用户详细信息表 t_user


| id(pk) | real name | email_address | login_id(FK+unique) |
| ------ | --------- | ------------- | ------------------- |
| 100    | 张三      | zhangsan@xxx  | 1                   |
| 200    | 李四      | lisi@xxx      | 2                   | 


---
### Summary


- 一对多：
	- 一对多两张表，多的表加外键
- 多对多：
	- 多对多，三张表，关系表两个外键
- 一对一：
	- 外键唯一

---
**Note!**

数据库设计三范式是理论上的 实践和理论有的时候有偏差。<br>
最终的目的是为了满足客户的需求，==有的时候会拿冗余换执行速度==
因为在SQL当中，表和表之间连接次数越多，效率越低 （笛卡尔积）

为了减少表的连接次数而存在数据冗余，这样做也是合理的，
并且对于开发人员来说，SQL语句的编写难度也会降低。


---

## Connection Pooling

Database [[Tree/connection-pooling\|connection-pooling]] is a way to reduce the cost of opening and closing connections by maintaining a “pool” of open connections that can be passed from database operation to database operation as needed. 