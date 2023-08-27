---
{"dg-publish":true,"permalink":"/tree/mysql/","tags":["CS/database"],"created":"2022-08-02T01:12:16.575+08:00","updated":"2023-08-27T13:23:13.161+08:00"}
---


# MySQL 

## MySQL命令

==MySQL命令不区分大小写==

### 退出
`exit`

### 终止一条语句的输入

 mysql不见分号不执行`\c`可用来终止一条语句的输入


### 查看mysql版本号
```sql
  select version();
```
### 查看mysql中有哪些数据库
```sql
  show databases;
```
 note! ==end with ';'==
 mysql默认带了4个数据库

### 查看当前使用的是哪个数据库
```sql
 select database();
```
### 使用数据库
 `use`

### 创建数据库
```sql
   create database name ; 
```
### 查看数据库下有哪些表
```sql
  show tables;
```
### 将sql文件中的数据导入数据库  

1. 创建数据库 
2. 使用数据库 
3. 执行命令  source+文件路径 (不加分号)


### 查看表中的数据 
```sql
  select * from table_name;  //查询某表中的所有数据
```
### 查看表的结构 不看数据
```sql
  desc table_name;  (describe的缩写)
```

 ### 使主键自动维护值 

```sql
id int primary key auto_increment,//从1开始，以1 自增 
```


##  MySQL 特性

### 日期

#### 默认日期格式

- 短日期 %Y-%m-%d 
- 长日期 %Y-%m-%d %h:%i:%s

#### 日期函数

NOW() 和 CURDATE() 是 MySQL 中的两个函数，都返回当前日期和时间，但有以下区别：

1. NOW() 返回当前日期和时间，包括时分秒，例如：2023-03-13 09:23:16

2. CURDATE() 只返回当前日期，不包括时间，例如：2023-03-13

### 自动维护主键

 在mysql中，一个字段同时被`not null`和`unique`约束 该字段自动成为主键字段

自动维护一个主键值
```sql
		drop table if exists t_vip;
		create table t_vip(
			id int primary key auto_increment, //auto_increment表示自增，从1开始，以1递增！
			name varchar(255)
		);
```
### MySQL中的索引

MySQL中，字段如果有`unique`约束，会自动创建[[Tree/SQL#索引\|索引]]对象

不管使用哪个存储引擎，索引在MySQL中都是以自平衡二叉树的形式存在

在MySQL中查看一个SQL语句是否使用了索引进行检索 

`explain`命令 

### 默认事务行为

mysql默认情况下支持自动提交事务,每执行一条DML语句，则提交一次
关闭mysql的自动提交机制关闭

先执行：`start transaction;`

### 默认隔离级别

MySQL中事务的默认隔离级别为[[Tree/SQL#repeatable read\|repeatable read]]

### 存储引擎

存储引擎是mysql中特有的一个术语（oracle中有，但不叫这个名字） 
**存储引擎实际上是一个表存储、组织数据的方式**,不同的存储引擎表存储数据的方式不同

#### 给表指定存储引擎

在建表的时候可以在最后小括号的`")"`的右边使用：

- `ENGINE`来指定存储引擎。
- `CHARSET`来指定这张表的字符编码方式。

mysql默认的存储引擎是：`InnoDB`
mysql默认的字符编码方式是：`utf8`

建表时指定存储引擎，以及字符编码方式 

#### MySQL 支持的存储引擎

用命令查看：

```sql 
show engines \G
```
版本不同，支持情况不同

#### 常用存储引擎

##### MyISAM

MyISAM管理的表具有以下特征：

- 使用三个文件表示每个表：
	- 格式文件一存储表结构的定义(mytable.frm)
	- 数据文件-存储表行的内容(ytable.MYD)
	- 索引文件一存储表上索引(mytable.MYI)

- 可被转换为压缩、只读表来节省空间(优点)
- 不支持事物机制，安全性低（缺点）

##### InnoDB

InnoDB 是MySQL默认的存储引擎
效率不是很高，不能转换为只读，不能压缩。
InnoDB存储引擎最主要的特点是：支持事务，支持数据库崩溃后自动恢复机制， 非常安全。

它管理的表具有下列主要特征：
- 每个InnoDB表在数据库目录中以.frm格式文件表示
- InnoDB 表空间 tablespace 被用于存储表的内容（表空间是一个逻辑名称，表空间存储数据+**索引**）
- 提供一组用来记录事务性活动的日志文件
- 用COMMIT(提交)、SAVEPOINT及ROLLBACK(回滚)支特事务处理
- 提供全ACID兼容
- 在MySQL服务器崩溃后提供自动恢复
- 多版本(VCC)和行级锁定
- 支持外键及引用的完整性，包括级联删除和更新

##### MEMORY 

使用MEMORY存储引擎的表，其数据存储在内存中，且行的长度固定，
这两个特点使得MEMORY存储引擎非常快。

MEMORY存储引擎管理的表具有下列特征：
- 在数据库目录内，每个表均以.frm格式的文件表示。
- 表数据及索引被存储在内存中。（目的就是快，查询快！）
- 表级锁机制。
- 不能包含TEXT或BLOB字段

MEMORY存储引擎以前被称为HEAP引擎
MEMORY引擎优点：查询效率最高
MEMORY引擎缺点：不安全，关机之后数据消失。因为数据和索引都是在内存中存储。