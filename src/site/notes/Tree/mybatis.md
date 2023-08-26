---
{"dg-publish":true,"permalink":"/tree/mybatis/","tags":["CS/programming-languages/java/java-frameworks"],"created":"2022-08-05T17:43:19.036+08:00","updated":"2023-08-27T03:49:40.296+08:00"}
---


**Notice** 

Advanced version see here: [[Tree/mybatis-plus\|mybatis-plus]]

# Introduction

Defination

MyBatis is a Java persistence framework that couples objects with stored procedures or SQL statements using an XML descriptor or annotations. MyBatis is free software that is distributed under the Apache License 2.0. MyBatis is a fork of iBATIS 3.0 and is maintained by a team that includes the original creators of iBATIS.

---

MyBatis是一款优秀的持久层框架，用于简化[[Tree/JDBC\|JDBC]]开发
- 持久层
	- 负责将数据到保存到数据库的那一层代码
	- JavaEE三层架构：表现层、业务层、持久层
- 框架
	- 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展

---
JDBC的缺点：

1. 硬编码
	- 注册驱动，获取连接
	- SQL语句

2. 操作繁琐
	- 手动设置参数
	- 手动封装结果集

---

==MyBatis免除了几乎所有的JDBC代码以及设置参数和获取结果集的工作==

---
# Basic

## Get Started

查询user表中所有数据 步骤：
1. 创建user表，添加数据
2. 创建模块，导入坐标
3. 编写MyBatis核心配置文件->替换连接信息 解决硬编码问题
4. 编写SQL映射文件->统一管理sql语句，解决硬编码问题
5. 编码
	1. 定义P0J0类
	2. 加载核心配置文件，获取SqlSessionFactory对象
	3. 获取SqlSession对象，执行SQL语句
	4. 释放资源

---

解决SQL映射文件的警告提示:

产生原因：IDEA和数据库没有建立连接，不识别表信息
解决方式：在IDEA中配置MySQL数据库连接


## Mapper

Mapper代理开发

目的:

- 解决原生方式中的硬编码
- 简化后期执行SQL

---

步骤:

使用Mapper代理方式完成入门案例

1. 定义与SQL映射文件同名的Mapper接口，并且==将Mapper接口和SQL映射文件放置在同一目录下==
	- 注意！在resources目录下创建包的目录结构用`/`作分隔符而不是`.`
2. 设置SQL映射文件的namespace属性为Mapper接口全限定名
3. 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id,并保持参数类型和返回值类型一致
4. 编码
	1. 通过SqlSession的getMapper方法获取Mapper接口的代理对象
	2. 调用对应方法完成sql的执行

---
细节：如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载
```xml
    <mappers>
        <!-- 加载sql映射文件     -->
        <!--  <mapper resource="com/mybatis/mapper/UserMapper.xml"/>-->

        <!-- Mapper代理方式        -->
        <package name="com.mybatis.mapper"/>
    </mappers>
```

## mybatis-config

MyBatis核心配置文件的顶层结构如下：
- configuration(配置)
- properties(属性)
- settings(设置)
- typeAliases(类型别名)
- typeHandlers(类型处理器)
- objectFactory(对象工厂)
- plugins(插件)
- environments(环境配置)
	- environment(环境变量)
		 - transactionManager(事务管理器)
		- dataSource(数据源)
-  databaseIdProvider(数据库厂商标识)
- mappers(映射器)

细节：**配置各个标签时，需要遵守前后顺序**

### 类型别名

**typeAliases**

类型别名是为Java类型设置的一个简短的名字 它只和XML配置有关，存在的意义仅在于减少类完全限定名的冗余，如下所示：
```xml
<typeAliases>
<typeAlias alias="user" type="cn.mybatis.domain.User"/>
</typeAliases>
```
这样配置时，user可以用在任何使用cn.mybatis.domain.User的地方。但如果类很多，每个类都这样配置一项显然很繁琐，下面给出另外一个解决方案

**指定特定包里面的类的别名**

```xml
<typeAliases>
	<package name="cn.mybatis.domain"/>
</typeAliases>
```
每一个在包cn.mybatis.domain中的Java Bean，在没有注解的情况下，会使用Bean的首字母==小写==的非限定类名来作为它的别名。比如cn.mybatis.domain.User的别名为user。若有注解，则别名为其注解值

```java
@Alias ("myuser")
public class User {
	...
}
```
可以指定一个包，让mybatis去扫描它，以cn.mybatis.domain这个包举例，mybatis扫描它时做了以下事情：这个包下的所有bean如果没有@Alias注解，mybatis会自动以这个类的首字母小写作为名称为它注册，也就是说cn.mybatis.domain.User以user注册，如果类上面有@Alias注解，就以这个注解的值myuser作为bean的别名



## 配置文件完成增删改查

要完成的功能列表清单：
1. 查询
	- 查询所有数据
	- 查看详情
	- 条件查询
1. 添加
2. 修改
	- 修改全部字段
	- 修改动态字段
3. 删除
	- 删除一个
	- 批量删除

---
MyBatisX插件

MybatisX是一款基于IDEA的快速开发插件
主要功能：
- XML和接口方法相互跳转
- 根据接口方法生成statement

### 查询

#### 查询所有数据
1. 编写接口方法：Mapper接口  `List<Brand>selectAll()`
	- 参数：无
	- 结果：List\<Brand>
2. 编写SQL语句：SQL映射文件

```xml
<select id="selectAll" resultType="brand">
	select * from tb_brand;
</select>
```
- 想要IDEA中出现SQL语句提示需配置SQL dialect（如mysql
3. 执行方法，测试

#### 查看详情

1. 编写接口方法：Mapper接口
	- 参数：id
		- 参数占位符
			1. #{} :   会将其替换为`？`，防止SQL注入
			2. ${} ：拼SQL  存在SQL注入问题
			3. 使用时机：
				- 参数传递的时候：#{}
				- 表名或者列名不固定的情况下：${} ,会存在SQL注入问题
		- 参数类型： parameterType:==可以省略==
		- 特殊字符处理：
			1. 转义字符
			2. CDATA区(输入CD出现提示)
	- 结果：Brand
2. 编写SQL语句：SQL映射文件
3. 执行方法，测试

#### 条件查询

##### 多条件查询
1. 编写接口方法：Mapper接口
- 参数：所有查询条件
	- 参数接收
		1. 散装参数：如果方法中有多个参数，需要使用@Param("SQL参数占位符名称")
		2. 对象参数：对象的属性名称要和对应的参数占位符一致
		3. Map集合参数：需要保证SQL中的参数名和map集合的键的名称对应

```xml
List<Brand> selectByCondition(@Param("status")int status,@Param("companyName")String
companyName,@Param("brandName")String brandName);

List<Brand> selectByCondition(Brand brand);

List<Brand> selectByCondition(Map map);
```


 - 结果：List\<Brand>

 
2. 编写SQL语句：SQL映射文件

```xml
<select id="selectByCondition"resultMap="brandResultMap">
select *
from tb_brand
where
status = #{status}
and company_name like #{companyName}
and brand_name like #{brandName}
</select>
```
3. 执行方法，测试

##### 多条件动态查询

SQL语句会随着用户的输入或外部条件的变化而变化，称为动态SQL

- 动态条件查询:
	- if:条件判断
	- test:逻辑表达式
	- 问题：
		- 恒等式
		- \<where>替换where关键字

MyBatis对动态SQL有很强大的支撑：
- if
- choose (when,otherwise)
- trim (where,set)
- foreach

##### 单条件动态查询 

从多个条件中选择一个

choose(when,otherwise):选择，类似于Java中的switch语句

```java
<select id="selectByConditionSingle"resultMap="brandResultMap">
select *
from tb brand
where
	<choose>          <!-- 类似于switch -->
		<when test="status=null">    <!--类似于case-->
			status =#{status}
		</when>
		<when test="companyName != null and companyName != '' "
			company_name like #{companyName}
		</when>
		<when test="brandName !null and brandName !='' "
			brand_name like #{brandName}
		</when>
		<otherwise>    <!--类似于default 也可用<where>省略此步-->
			1=1
		</otherwise>
	</choose>
</select>
```

### 添加

#### 基础添加

1. 编写接口方法：Mapper接口 `void add(Brand brand);`
	- 参数：除了id之外的所有数据
	- 结果：void
1. 编写SQL语句：SQL映射文件


```xml
<insert id="add">
	insert into tb_brand (brand_name,company_name,ordered,description,status)
	values (#{brandName},#{companyName},#{ordered},#{description},#{status});
</insert>
```

3. 执行方法，测试
- MyBatis事务：
	- openSession():默认开启事务，进行增删改操作后需要使用sqlSession.commit();手动提交事务
	- openSession(true):可以设置为自动提交事务（关闭事务）

#### 主键返回

在数据添加成功后，需要获取插入数据库数据的主键

比如：添加订单和订单项:
1. 添加订单
2. 添加订单项，订单项中需要设置所属订单的id
```xml
<insert id="addOrder"useGeneratedKeys="true"keyProperty="id">
	insert into tb_order(payment,payment_type,status) values {#payment},#{paymentType},#{status});
</insert>
```
```xml
<insert id="addOrderItem">
	insert into tb_order_item (goods_name,goods_price,count,order_id)
values (#{goodsName},#{goodsPrice},#{count},#{orderld});
</insert>
```

返回添加数据的主键:

```xml
<insert useGeneratedKeys="true"keyProperty="id">
```

### 修改

#### 修改全部字段

1. 编写接口方法：Mapper接口 `void update(Brand brand);`
- 参数：所有数据
- 结果：void

2. 编写SQL语句：SQL映射文件

```xml
<update id="update">
update tb_brand
set brand_name = #{brandName},
company_name = #{companyName},
ordered = #{ordered},
description = #{description},
status = #{status}, 
where id =  #{id};
</update>
```
3. 执行方法，测试

#### 修改动态字段

1. 编写接口方法：Mapper接口
	- 参数：部分数据，封装到对象中
	- 结果：void
2. 编写SQL语句：SQL映射文件
```xml
<update id="update">
	update tb_brand
	<set>  
		<if test="brandName != null and brandName != '' ">
			brand_name = #{brandName},
		</if>
		<if test="companyName != null and companyName !='' ">
			company_name = #{companyName},
		</if>
		<if test="ordered != null">
			ordered = #{ordered},
		</if>
		<if test="description != null and description != ''">
			description = #{description},
		</if>
		<if test="status != null">
			status = #{status},
		</if>
	</set>
		where id = #{id};
</update>
```

3. 执行方法，测试

### 删除

#### 删除一个

1. 编写接口方法：Mapper接口 `void deleteByld(int id);`
	- 参数：id
	- 结果：void
2. 编写SQL语句：SQL映射文件
3.
```xml
<delete id="deleteById">
delete from tb_brand where id = #{id}
</delete>
```
3. 执行方法，测试

#### 批量删除 


1. 编写接口方法：Mapper接口 
`void deleteBylds(@Param("ids")int[] ids);`
	- 参数：id数组
	- 结果：void
3. 编写SQL语句：SQL映射文件

```xml
<delete id="deleteBylds">
	delete from tb brand
	where id in
	<foreach collection="ids"item="id"separator=","open="("close=")">
		#{id}
	</foreach>
</delete>
```
3. 执行方法，测试

---

注意 ：==MyBatis 会将数组参数 封装为一个Map集合==
- 默认key ：   array = 数组
- 使用@Param注解改变map集合默认key的名称

### Mybatis参数传递

MyBatis接口方法中可以接收各种各样的参数，MyBatis底层对于这些参数进行不同的封装处理方式
 
- 单个参数：
1. POJO类型:直接使用，属性名和和参数名占位符名称一致
2. Map集合：key名和和参数名占位符名称一致
3. Collection:封装为Map集合 可以使用@Param注解，替换Map集合中默认的键名
	  - map.put("argo",collection集合);
	  - map.put("collection",collection集合);
4. List:封装为Map集合 可以使用@Param注解，替换Map集合中默认的键名
	- map.put("argo",List集合)；
	- map.put("collection",list集合)；
	- map.put("List",List集合)；
5. Array:封装为Map集合 可以使用@Param注解，替换Map集合中默认的键名 
	- map.put("arg0",数组)：
	- map.put("array",数组)；
6. 其他类型：直接使用

- 多个参数：封装为Map集合,可以使用@Param注解，替换Map集合中默认的键名
	- map.put("arg0",参数值1)
	- map.put("param1",参数值1)
	- map.put("agr1",参数值2)
	- map.put("param:2",参数值2)

```java
User select(@Param("username")String username,@Param("password")String password);
```
```xml
<select id="select"resultType="user">
select
from tb_user
where
username = #{username}
and password = #{password};
</select>
```
==MyBatis提供了ParamNameResolver类来进行参数封装== 


## 注解完成增删改查

- 查询：@Select
- 添加：@Insert
- 修改：@Update
- 删除：@Delete

```java
@Select("select from tb_user where id = #{id}")
public User selectByld(int id);
```

**注解完成简单功能 配置文件完成复杂功能**

