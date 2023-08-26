---
{"dg-publish":true,"permalink":"/tree/jdbc/","tags":["CS/programming-languages/java"],"created":"2022-08-02T17:31:16.042+08:00","updated":"2023-08-27T03:49:06.473+08:00"}
---


# JDBC 
Defination:
Java Database Connectivity (JDBC) is an application programming interface (API) for the programming language Java, which defines how a client may access a [[Tree/database\|database]]. 

## Introduction

JDBC本质：
- 官方 (sun公司)定义的一套操作所有关系型数据库的规则，即接口
- 各个数据库厂商去实现这套接口，提供数据库驱动jar包
- 我们可以使用这套接口(JDBC)编程，真正执行的代码是驱动jar包
中的实现类

JDBC好处：

- 各数据库厂商使用相同的接口，java代码不需要针对不同数据库分别开发
- 可随时替换底层数据库，访问数据库的java代码基本不变

## Get Started

0. 首先创建project ,module . import driver (jar file ) as library.

```java
package com.jdbclearn.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

/*
 * JDBC 快速入门
 */

public class JDBCDemo {
  public static void main(String[] args) throws Exception {
    // 1.注册驱动   (现在已经可以略去此步骤）
    Class.forName("com.mysql.cj.jdbc.Driver");

    // 2.获取连接
    String url = "jdbc:mysql://127.0.0.1:3306/bjpowernode";
    String username = "root";
    String password = "20220711";
    Connection conn = DriverManager.getConnection(url, username, password);

    // 3.定义SQL
    String sql = "update emp set sal = 8000 where empno = 7369";

    // 4. 获取执行sql的对象 Statement
    Statement stmt = conn.createStatement();

    // 5. 执行SQL

    int count = stmt.executeUpdate(sql); // 返回受影响的行数

    // 6. 处理结果
    System.out.println(count);

    // 7. 释放资源
    stmt.close();
    conn.close();
  }
}
```

## API Details

### DriverManager

- DriverManager(驱动管理类)作用：
1. 注册驱动

2. 获取数据库连接:

```java
static Connection
getconnection(string url,string user,string password)
```
参数

1. url:连接路径

- 语法：jdbc:mysql::lp地址（域名）：端口号/数据库名称？参数键值对1&参数键值对2 ...
- 示例：jdbc:mysql://127.0.0.1：3306/db1
- 细节：
	- 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称？参数键值对
	- 配置useSSL=false参数，禁用安全连接方式，解决警告提示

2. user: 用户名
3. password: 密码

### Connection

Connection(数据库连接对象)作用：
1. 获取执行SQL的对象

- 普通执行SQL对象
Statement createStatement()
- 预编泽SQL的执行SQL对象：防止SQL注入
PreparedStatement prepareStatement(sql)
 - 执行存储过程的对象
CallableStatement prepareCall(sql)

2. [[Tree/SQL#事务 transaction\|事务]]管理

- MySQL事务管理

	- 开启事务：BEGIN;/START TRANSACTION;
	- 提交事务：COMMIT;
	- 回滚事务：ROLLBACK;
	- MySQL默认自动提交事务

- JDBC事务管理：Connection:接口中定义了3个对应的方法
	- 开启事务：setAutoCommit(boolean autoCommit):true为自动提交事务；false为手动提交事务，即为开启事务
	- 提交事务：commit()
	- 回滚事务：rollback(）

### Statement

Statement作用：

**执行SQL语句**

- `int executeUpdate(sql)`:
	- 执行DML、DDL语句
	- 返回值：
		- 1. DML语句影响的行数
		- 2. DDL语句执行后，执行成功也可能返回0

- `ResultSet executeQuery(sql)`:
	- 执行DQL语句
	- 返回值：ResultSet结果集对象

#### ResultSet

- ResultSet(结果集对象)作用：
	- 封装了DQL查询语句的结果
	- ResultSet stmt.executeQuery(sql):执行DQL语句，返回ResultSet对象

- 获取查询结果
	- boolean next():
		1. 将光标从当前位置向前移动一行
		2. 判断当前行是否为有效行
	- 返回值：
		- true:有效行，当前行有数据
		- false:无效行，当前行没有数据


xxx  getXxx(参数)：获取数据
- xxx:数据类型；如：int getInt(参数)；String getString(参数)
- 参数：
	- int:列的编号，==从1开始==
	- String:列的名称

 使用步骤：
1. 游标向下移动一行，并判断该行否有数据：next()
2. 获取数据：getXxx(参数)

```java
//循环判断游标是否是最后一行末尾
while(rs.next()){
//获取数据
rs.getXxx(参数)；
}
```

## PreparedStatement

- PreparedStatement作用：
	- 预编译SQL语句并执行：预防SQL注入问题
		- SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法。

预编译SQL并执行SQL语句:
1. 获取PreparedStatement对象
```java
//SQL语句中的参数值，使用？占位符替代
String sql "select from user where username and password ?"
//通过Connection.对象获取，并传入对应的sgl语句
PreparedStatement pstmt conn.prepareStatement(sql);
```
2. 设置参数值

PreparedStatement对象：setXxx(参数1，参数2)：给？赋值
- Xx:数据类型；如setInt(参数1，参数2)
- 参数：
	- 参数1：？的位置编号，从1开始
	- 参数2：？的值
3. 执行SQL
- executeUpdate();/executeQuery(); : 不需要再传递sql

---

- PreparedStatement好处：
1. 预编译SQL,性能更高
2. 防止SQL注入：将敏感字符进行转义

- PreparedStatement预编译功能开启：`useServerPrepStmts=true`
- 配置MySQL执行日志（重启mysql服务后生效）

```
log-output=FILE
general-log=1
setS
general_log_file="D:\mysql.log"
slow-query-log=1
slow_query_.log_file=“D\mysql_slow.log”
long_query_time=2
```
- PreparedStatement原理：
	1. 在获取PreparedStatement对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
	2. 执行时就不用再进行这些步骤了，速度更快
	3.  如果sql模板一样，则只需要进行一次检查、编译