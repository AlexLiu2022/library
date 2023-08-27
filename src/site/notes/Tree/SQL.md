---
{"dg-publish":true,"dg-path":"  SQL.md","permalink":"/sql/","tags":["CS/database"],"created":"2022-08-02T01:25:27.426+08:00","updated":"2023-08-27T03:45:46.598+08:00"}
---


# SQL

## 数据类型

### 字符串

- `varchar`

- 可变长度的字符串会根据实际的数据长度动态分配空间

- 优点：节省空间
-  缺点：动态分配造成效率低

- `char`

	- 定长字符串

	- 优点：速度快
	- 缺点： 可能浪费空间   

	- 最长255个字符   

### 数字

- `tinyint` (用tinyint(1) 表示boolean)
- `int`（最长11位）
- `bigint`
- `float`
- `double`


### 日期

- `date`

	- 短日期类型 只包括年月日信息

- `datetime`

	- 长日期类型 包括年月日时分秒信息

- now()函数获取系统当前时间 并且获取的时间带有时分秒信息 是datetime类型的 


### clob

- character large object 字符大对象 最多可以存储4G的字符串


### blob

- 二进制大对象  专门用来存储图片、视频、声音等流媒体数据
- 往blob类型的字段插入数据的时候，例如插入一个图片、视频等，需要使用IO流


## DQL数据查询语言

凡是带有`select`关键字的都是查询语句

### 关键字的执行顺序

`from.`.. =>`where`...=>`group by`...=>`having`... => `select`... =>`order by`...=>`limit`...

 ### 简单查询

#### 查询一个字段

`select field_name from table_name;`

#### 查询多个字段

use commas to separate them

#### 查询所有字段

write all the fields 

use asterisk

- 这种方式的缺点

	- 1 效率低
	- 2 可读性差

#### 给查询的列起别名

 use keyword `as` (==or just add a space==)
 
 if there are spaces in the alias, enclosed in single quotation marks
 
 ==在所有数据库中 字符串统一用单引号括起来==


**字段可以使用数学表达式**

```sql
select ename,sal*12 as yearsal from emp;
```
### 条件查询

`select fields from tables where conditions` 

有以下条件

#### 等于 = ；不等于！= / <>  ；<= ,<

#### between ... and ... 

等价于(>= ... and  <=... )  

==必须左小右大 between and是闭区间，包括两端的值==

#### is null/ is not null 

==数据库中的null代表什么也没有，它不是一个值==，所以不能使用等号衡量。需要使用is null/ is not null

#### and 并且

and 优先级比or 高

#### or 或者

想让or比and先执行 需要加小括号

#### in 包含 

相当于多个or

```sql
select empno,ename,job from emp where job in('MANAGER', 'SALESMAN');
```
not in ：不在这个范围中

>注意：in不是一个区间 in后面跟的是具体的值

### 模糊查询

`like` 

支持% 或 _匹配

####  %匹配任意多个字符 
####  _匹配任意一个字符

若要匹配带以上特殊符号的 使用转义字符\

#### 例子

- 找出名字以T结尾的？

	`select ename from  emp where ename like '%T'；`

- 找出名字以K开始的？

	`select ename from emp where ename like 'K%'；`
	
- 找出第二个字每是A的？
	
	 `select ename from emp where ename like '_A%';`

- 找出第三个字母是R的？
	
	 `select ename from emp where ename like '__R%'：`

 #### 排序

#### order by

 默认是升序

 指定升序 `asc`

  指定降序

 `order by field desc (end)`

#### 多个字段排序

- use commas to separate them 在前的字段起主导 相同时依据后方的字段排序

#### 根据字段的位置排序

- `order by 2` 按照查询结果的第2列排序 

### 数据处理函数

（单行处理函数)  特点：一个输入对应一个输出

#### lower 转小写 upper 转大写

```sql
select lower(ename) as ename from emp;
```
#### substr 取子串

 substr(被截取的字符串，起始下标，截取的长度)  **注意起始下标是1不是0**

#### contact 字符串拼接
####  lenth 取长度
####  trim 去空格
####  round 四舍五入

`select`后面可以跟某个表的字段名（可以等同看做变量名），也可以跟字面值（数据）
```sql
		select 21000 as num from dept;
		+-------+
		| num   |
		+-------+
		| 21000 |
		| 21000 |
		| 21000 |
		| 21000 |
		+-------+
```
- round(字段或字面值，数字）
```sql
select round(1236.567, 0) as result from emp; //保留整数位
```
- 数字 = 0 保留到整数
- 1 保留到小数点后一位
- -1 保留到十位

#### rand生成随机数
```sql
select round(rand()*100,0) from emp; // 100以内的随机数
		+---------------------+
		| round(rand()*100,0) |
		+---------------------+
		|                  76 |
		|                  29 |
		|                  15 |
		|                  88 |
		|                  95 |
		|                   9 |
		|                  63 |
		|                  89 |
		|                  54 |
		|                   3 |
		|                  54 |
		|                  61 |
		|                  42 |
		|                  28 |
		+---------------------+
```
####  ifnull 空处理函数

在所有数据库中 只要有`null`参与的数学运算最终结果都是`null`


**ifnull(数据，数据为null时被当作哪个值)**

补助为NULL的时候，将补助当做0
```sql
select ename, (sal + ifnull(comm, 0)) * 12 as yearsal from emp;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 22800.00 |
| WARD   | 21000.00 |
| JONES  | 35700.00 |
| MARTIN | 31800.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| SCOTT  | 36000.00 |
| KING   | 60000.00 |
| TURNER | 18000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| FORD   | 36000.00 |
| MILLER | 15600.00 |
+--------+----------+
```


#### case... when ... then...  when... then... else ...end

当员工的工作岗位是MANAGER的时候，工资上调10%，当工作岗位是SALESMAN的时候，工资上调50%,其它正常。
		（注意：不修改数据库，只是将查询结果显示为工资上调）
```sql		
		select 
			ename,
			job, 
			sal as oldsal,
			(case job when 'MANAGER' then sal*1.1 when 'SALESMAN' then sal*1.5 else sal end) as newsal 
		from 
			emp;
		
		+--------+-----------+---------+---------+
		| ename  | job       | oldsal  | newsal  |
		+--------+-----------+---------+---------+
		| SMITH  | CLERK     |  800.00 |  800.00 |
		| ALLEN  | SALESMAN  | 1600.00 | 2400.00 |
		| WARD   | SALESMAN  | 1250.00 | 1875.00 |
		| JONES  | MANAGER   | 2975.00 | 3272.50 |
		| MARTIN | SALESMAN  | 1250.00 | 1875.00 |
		| BLAKE  | MANAGER   | 2850.00 | 3135.00 |
		| CLARK  | MANAGER   | 2450.00 | 2695.00 |
		| SCOTT  | ANALYST   | 3000.00 | 3000.00 |
		| KING   | PRESIDENT | 5000.00 | 5000.00 |
		| TURNER | SALESMAN  | 1500.00 | 2250.00 |
		| ADAMS  | CLERK     | 1100.00 | 1100.00 |
		| JAMES  | CLERK     |  950.00 |  950.00 |
		| FORD   | ANALYST   | 3000.00 | 3000.00 |
		| MILLER | CLERK     | 1300.00 | 1300.00 |
		+--------+-----------+---------+---------+
```
#### format 数字格式化

#### str_to_date

- 将字符串`varchar`类型转换为`date`类型 

- 语法格式 `str_to_date('字符串日期'，'日期格式')`
- 日期格式

	- %Y年 %m月 %d日 %h时 %i分 %s秒

- 如果字符串是 '%Y-%m-%d' 格式 不需要该函数， 能够自动类型转换  

#### date_format 

- 将`date`类型转换为具有一定格式的`varchar`类型
- `date_format(field,'日期格式')` 

### 分组函数
 
 （多行处理函数）

 5个

- `count` 计数
- `sum` 求和
- `avg` 平均数
- `max` 最大值
- `min` 最小值

必须先分组才能使用 ==如果没有对数据进行分组 整张表默认为一组== 

注意

- 分组函数自动忽略null 不需要对null进行处理
- 分组函数中count * 和count 具体字段的区别

- count具体字段 表示统计该字段下不为null的元素总数
- count * 统计表中总行数 因为每一行数据中不可能每一个数据都是null

- **分组函数不能直接使用在where子句中,因为where执行顺序更前面**
- 所有分组函数可以组合起来一起用

### 分组查询

####  group by 分组 

- 有`group by` 语句时`select`后只能跟参与分组的字段与分组函数

出每个工作岗位的工资和
	
实现思路：按照工作岗位分组，然后对工资求和。

```sql		
select 
	job,sum(sal)
from
	emp
group by
	job;

+-----------+----------+
| job       | sum(sal) |
+-----------+----------+
| ANALYST   |  6000.00 |
| CLERK     |  4150.00 |
| MANAGER   |  8275.00 |
| PRESIDENT |  5000.00 |
| SALESMAN  |  5600.00 |
+-----------+----------+
```
以上语句的执行顺序:
	先从emp表中查询数据。
	根据job字段进行分组。
	然后对每一组的数据进行sum(sal)

#### having

- 对分完组的数据进一步过滤 只能和`group by` 联合使用 不能代替`where`

- `where` 和 `having`都能使用时优先使用`where` 效率更高

### 查询结果去重

关键字`distinct`

 `distinct` 只能出现在所有字段的前方(分组函数中可用)
```sql
// distinct出现在job,deptno两个字段之前，表示两个字段联合起来去重。
	mysql> select distinct job,deptno from emp;
	+-----------+--------+
	| job       | deptno |
	+-----------+--------+
	| CLERK     |     20 |
	| SALESMAN  |     30 |
	| MANAGER   |     20 |
	| MANAGER   |     30 |
	| MANAGER   |     10 |
	| ANALYST   |     20 |
	| PRESIDENT |     10 |
	| CLERK     |     30 |
	| CLERK     |     10 |
	+-----------+--------+
```
### 连接查询

#### 分类

- 根据语法的年代 

	- SQL92
	
		- 缺点：结构不够清晰 表连接的条件和后续筛选的条件都放在where后面
	
	- SQL99
	
		- 优点：表连接的条件是独立的 连接之后如果需要进一步筛选，再往后继续添加where

- 根据表连接的方式分类

- 内连接

	- 等值连接

		- 连接条件是等量关系式

	- 非等值连接

		- 条件不是一个等量关系

	- 自连接

		- 一张表看作两张表

		- 表之间没有主次关系

- 外连接

	- 左外连接
	- 右外连接

- 全连接
- 多张表的连接


#### 笛卡尔积现象
当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是两张表条数的乘积，即笛卡尔积现象。

避免笛卡尔积现象

连接时加条件，满足条件的记录被筛选出来

```sql	
	select 
		ename,dname 
	from 
		emp, dept
	where
		emp.deptno = dept.deptno;
	
	select 
		emp.ename,dept.dname 
	from 
		emp, dept
	where
		emp.deptno = dept.deptno;
	
	// 表起别名。很重要。效率问题。
	select 
		e.ename,d.dname 
	from 
		emp e, dept d
	where
		e.deptno = d.deptno; //SQL92语法。

	+--------+------------+
	| ename  | dname      |
	+--------+------------+
	| CLARK  | ACCOUNTING |
	| KING   | ACCOUNTING |
	| MILLER | ACCOUNTING |
	| SMITH  | RESEARCH   |
	| JONES  | RESEARCH   |
	| SCOTT  | RESEARCH   |
	| ADAMS  | RESEARCH   |
	| FORD   | RESEARCH   |
	| ALLEN  | SALES      |
	| WARD   | SALES      |
	| MARTIN | SALES      |
	| BLAKE  | SALES      |
	| TURNER | SALES      |
	| JAMES  | SALES      |
	+--------+------------+
```
最终查询的结果条数是14条，但是匹配的过程中，匹配的次数还是56次，
只不过进行了四选一

注意：通过笛卡尔积现象得出，表的连接次数越多效率越低，尽量避免表的
连接次数


#### 内连接

内连接：（A和B连接，AB两张表没有主次关系）
内连接的特点：完全能够匹配上条件的数据查询出来

##### 等值连接


案例：查询每个员工所在部门名称，显示员工名和部门名
emp e和dept d表进行连接 条件是：e.deptno = d.deptno
SQL92语法：
```sql
	select 
		e.ename,d.dname
	from
		emp e, dept d
	where
		e.deptno = d.deptno;		
```
sql92的缺点：结构不清晰，表的连接条件，和后期进一步筛选的条件，都放到了where后面

SQL99语法：

```sql
select 
	e.ename,d.dname
from
	emp e
join
	dept d
on
	e.deptno = d.deptno;


//inner可以省略（带着inner可读性更好）
select 
	e.ename,d.dname
from
	emp e
inner join
	dept d
on
	e.deptno = d.deptno; // 条件是等量关系，所以被称为等值连接。
```
	
sql99优点：表连接的条件是独立的，连接之后，如果还需要进一步筛选，再往后继续添加`where`

SQL99语法：

```sql
		select 
			...
		from
			a
		join
			b
		on
			a和b的连接条件
		where
			筛选条件
```

##### 非等值连接
```sql
select 
	e.ename, e.sal, s.grade
from
	emp e
inner join
	salgrade s
on
	e.sal between s.losal and s.hisal;

+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SMITH  |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+
```

##### 自连接

一张表看作两张表

#### 外连接

外连接（右外连接）：
```sql
select 
	e.ename,d.dname
from
	emp e 
right join 
	dept d
on
	e.deptno = d.deptno;

// outer是可以省略的，带着可读性强。
select 
	e.ename,d.dname
from
	emp e 
right outer join 
	dept d
on
	e.deptno = d.deptno;
```
right表示将join关键字右边的这张表看成主表，主要是为了将这张表的数据全部查询出来，捎带着关联查询左边的表。
在外连接当中，两张表连接，产生了主次关系。

外连接（左外连接）：

```sql
select 
	e.ename,d.dname
from
	dept d 
left join 
	emp e
on
	e.deptno = d.deptno;

// outer是可以省略的，带着可读性强。
select 
	e.ename,d.dname
from
	dept d 
left outer join 
	emp e
on
	e.deptno = d.deptno;
```

带有right的是右外连接，又叫做右连接。
带有left的是左外连接，又叫做左连接。
任何一个右连接都有左连接的写法。
任何一个左连接都有右连接的写法。

```sql
+--------+------------+
| ename  | dname      |
+--------+------------+
| CLARK  | ACCOUNTING |
| KING   | ACCOUNTING |
| MILLER | ACCOUNTING |
| SMITH  | RESEARCH   |
| JONES  | RESEARCH   |
| SCOTT  | RESEARCH   |
| ADAMS  | RESEARCH   |
| FORD   | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| TURNER | SALES      |
| JAMES  | SALES      |
| NULL   | OPERATIONS |
+--------+------------+
```

#### 多表连接

```sql
select 
	...
from
	a
join
	b
on
	a和b的连接条件
join
	c
on
	a和c的连接条件
right join
	d
on
	a和d的连接条件

```

一条SQL中内连接和外连接可以混合

### 子查询

`select` 语句中嵌套`select`语句 被嵌套的`select`语句称为子查询

 子查询可以出现在 `select` `where` `from` 子句中
 
 ```sql
select ename,sal from emp where sal > (select min(sal) from emp);
``` 

 `from`子句中的子查询 可以将子查询的查询结果当作一张临时表

```sql
	select 
	t.*, s.grade
		from
			(select job,avg(sal) as avgsal from emp group by job) t
		join
			salgrade s
		on
			t.avgsal between s.losal and s.hisal;
		
		+-----------+-------------+-------+
		| job       | avgsal      | grade |
		+-----------+-------------+-------+
		| CLERK     | 1037.500000 |     1 |
		| SALESMAN  | 1400.000000 |     2 |
		| ANALYST   | 3000.000000 |     4 |
		| MANAGER   | 2758.333333 |     4 |
		| PRESIDENT | 5000.000000 |     5 |
		+-----------+-------------+-------+
```

 `select` 后出现的子查询只能返回一条结果  多于1条就报错

```sql
select 
		e.ename,e.deptno,(select d.dname from dept d where e.deptno = d.deptno) as dname 
	from 
		emp e;

	+--------+--------+------------+
	| ename  | deptno | dname      |
	+--------+--------+------------+
	| SMITH  |     20 | RESEARCH   |
	| ALLEN  |     30 | SALES      |
	| WARD   |     30 | SALES      |
	| JONES  |     20 | RESEARCH   |
	| MARTIN |     30 | SALES      |
	| BLAKE  |     30 | SALES      |
	| CLARK  |     10 | ACCOUNTING |
	| SCOTT  |     20 | RESEARCH   |
	| KING   |     10 | ACCOUNTING |
	| TURNER |     30 | SALES      |
	| ADAMS  |     20 | RESEARCH   |
	| JAMES  |     30 | SALES      |
	| FORD   |     20 | RESEARCH   |
	| MILLER |     10 | ACCOUNTING |
	+--------+--------+------------+

	//错误：ERROR 1242 (21000): Subquery returns more than 1 row
	select 
		e.ename,e.deptno,(select dname from dept) as dname
	from
		emp e;
````

### union

合并查询结果集

- union相比表连接效率高 把乘法运算转换为加法运算 减少匹配次数
- union在进行结果集的合并时 要求两个结果集列的数目相同 数据类型也相同

```sql
select ename,job from emp where job = 'MANAGER'
	union
select ename,sal from emp where job = 'SALESMAN';
```

---

`UNION`和`UNION ALL`都是用于将两个或多个SELECT语句的结果组合在一起。区别在于：

1. UNION会自动去重，而UNION ALL不会。也就是说，如果两个SELECT语句的结果有相同的行，UNION会只返回其中的一个，而UNION ALL会返回两个。

2. 由于UNION需要执行去重操作，所以相对于UNION ALL而言，它的执行速度可能会慢一些。



### limit

- `limit`将查询结果集的一部分取出来，通常使用在分页查询当中
- `limit startIndex length`

- 缺省用法 `limit 5` 取前五

- mysql中 `limit`在`order by `后执行

#### 分页

- 公式` limit (pageNo - 1)*pageSize, pageSize`

## DDL数据定义语言 

凡是带有`create ` `drop` ` alter`的 都是DDL DDL主要操作表的结构 不是数据

`create` 新建 

 ### 表的创建

- `create table table_name (field type ,field type ,field type ...);`

```sql
create table t_student(
		no int,
		name varchar(32),
		sex char(1),
		age int(3),
		email varchar(255)
	);	
```
- ==可以在建表时指定默认值 field type default default-value==

### 快速创建表 

 将一个查询结果当作一张表新建 可以完成表的快速复制 `create table new_table_name  as select ...`

### 约束

直接添加到字段后叫列级约束，没有添加到列后叫表级约束

约束有以下几类：

#### 非空约束

`not null`

```sql
drop table if exists t_vip;
	create table t_vip(
		id int,
		name varchar(255) not null  // not null只有列级约束，没有表级约束！
	);
	insert into t_vip(id,name) values(1,'zhangsan');
	insert into t_vip(id,name) values(2,'lisi');

	insert into t_vip(id) values(3);
	ERROR 1364 (HY000): Field 'name' doesn't have a default value
```

#### 唯一性约束
 `unique`
- `unique`约束的字段不能重复，但是可以为`null`
- 两个字段联合唯一

	- `unique(field_1, field_2)`

```sql
drop table if exists t_vip;
			create table t_vip(
				id int,
				name varchar(255),
				email varchar(255),
				unique(name,email) // 约束没有添加在列的后面，为表级约束。
			);
```
#### 主键约束

主键 `primary key` 简称pk

主键值是每一行记录的唯一标识

任何一张表都应该有主键 ，没有主键，表无效

主键的特征： not null + unique 

主键值建议使用

- `int` /`bigint`  
- `char`                     等类型 ==不建议使用`varchar`做主键==

主键值一般都是数字 一般都是定长的 

##### 相关术语 
- 主键约束：一种约束
- 主键字段：添加了主键约束的主键字段
- 主键值：每一个主键字段的值 

##### 主键分类方式1

- 一个字段做主键叫单一主键 
- 两个字段联合做主键叫复合主键 

（==实际开发中不建议使用复合主键==）  

一张表主键约束只能有一个 


##### 主键分类方式2

- 自然主键 ：主键值是一个自然数 和业务无关系
- 业务主键 ：主键值和业务紧密相连，例如：银行卡账号

自然主键使用较多 业务发生变化时 可能会改变主键值  ==不建议使用业务主键==
  

#### 外键约束

`foreign key` 简称FK

写法 

```sql
foreign key(field1) references table_name(field2 )
```

被引用的外键字段不一定是主键，但至少受到`unique`约束

外键值可以为NULL

#### 检查约束

- check (mysql 不支持 oracle支持）

### drop 删除

#### 删除表

- `drop table_name;`

	- 当这张表不存在时会报错

- `drop table if exists table_name`;

#### alter 修改

实际开发中表创建好后很少会修改，实在要改可借助`navicat`等工具实现

##  DML数据操作语言

凡是对表中数据进行增删改的都是DML insert delete update

### insert 插入数据 

- `insert into table_name (field1, field2, ...) values(value1, value2, ...) ;`
- insert 语句只要执行成功 一定会多一条记录 没有给其他字段指定值的话 默认字段是null
- 可以省略字段名 ==等价于都写上==
- 一次插入多条记录 以逗号隔开不同的值
- 将查询结果插入到一张表中

	- `insert into table_name select  ...`

### update 更新数据

- `update table_name set field1 = value1, field2 = value2 ... where conditions;` 没有条件会导致所有数据全部更新

### delete 删除数据 

- `delete from table_name where conditions`

#### 快速删除

- `delete * from table_name`  删除数据的方式比较慢

	- 原理：数据被删除了但数据在硬盘上的真实存储空间不会被释放！！

	- 优点：可回滚
	- 缺点：删除效率低

- `truncate`语句删除数据（DDL）

	- 物理删除，表被一次截断
	- `truncate table table_name;`


## TCL 事务控制语言

### 事务 transaction

一个事务是一个完整的业务逻辑，是一个最小的工作单元。不可再分。

一个完整的业务逻辑举例：

  假设转账，从A账户向B账户中转账10000
- 将A账户的钱减去10000 (update语句)
- 将B账户的钱加上10000（update语句)

这两个update语句是一个最小的工作单元，要求**必须同时成功或者同时失败**，不可再分，才能保证钱是正确的。

只有DML语句（update delete insert)与事务有关 （操作一旦涉及到数据的增删改，就一定要考虑到安全问题）

做某件事的时候，需要多条DML语句共同联合起来才能完成，所以需要事务的存在。==事务就是批量的DML语句同时成功，或者同时失败==

### 事务实现原理

事务是怎么做到多条D匹语句同时成功和同时失败的呢？

[[Tree/mysql#InnoDB\|InnoDB]]存储引擎：提供一组用来记录事务性活动的日志文件

事务开启了：
insert ··· 
insert ···
insert ···
delete ···
update ···
update ···
update ···
事务结束了！

在事务的执行过程中，每一条DML操作都会记录到"事务性活动的日志文件"中
在事务的执行过程中，我们可以提交事务，也可以回滚事务。

- 提交事务：
清空事务性活动的日志文件，将数据全部彻底特久化到数据库表中。
提交事务标志着，事务的结束。并且是一种全部成功的结束。
- 回滚事务：
将之前所有的D血操作全部撤销，并且清空事务性活动的日志文件
回滚事务标志着，事务的结束。并且是一种全部失败的结束。

### 事务提交

`commit`
 
### 事务回滚

`rollback`

### 事务的特性

- A 原子性
	- 说明事物是最小的工作单元 不可再分
- C 一致性
	- 同一个事物中 所有操作必须同时成功 同时失败 以保证数据的一致性 
- I 隔离性
	- A事务和B事务之间具有一定的隔离
- D 持久性
	- 事务最终结束的一个保障。事务提交，相当于将没有保存到硬盘上的数据保存到硬盘上

#### 事物隔离级别

#####  read uncommitted
（没有提交就读到了）
最低的隔离级别 事务A可以读取到事务B未提交的数据

存在的问题 ： **Dirty Read** 现象

这种隔离级别通常是理论上的，大多数的数据库隔离级别都是二档起步

##### read committed
（提交后才能读到）
事务A只能读取到事务B提交之后的数据

解决了 **Dirty Read** 现象
存在的问题 ： 不可重复读取数据

这种隔离级别读到的数据比较真实

是Oracle数据库的默认隔离级别

#####  repeatable read 
（提交了也读不到）
事务A开启之后，每一次在事务A中读取到的数据都是一致的。即使事务B将数据已经修改并且提交，事务A读取到的数据还是没有发生改变.

存在的问题 ：幻影读  读取到的数据不够真实

是MySQL中的默认隔离级别

##### serializable

最高的事物隔离级别，解决了所有问题，但效率最低。

表示事物排队，不能并发

## DCL数据控制语言

### 授权 grant 

### 撤销权限 revoke

## index索引

### 索引

索引是在数据库表的字段上添加的，为了提高查询效率的一种机制。
索引相当于一本书的目录，为了缩小扫描范围而存在。
一张表的一个字段可以添加一个索引，多个字段联合起来也可以添加索引。

在任何数据库中主键都会自动添加索引对象

### 索引应用场景

- 数据量庞大
- 该字段总是以条件的形式出现在`where`后面（总是被扫描）
- 该字段很少DML操作
	- 因为DML操作后，索引需要重新排序

**不要随意添加索引，因为索引是需要维护的**

### 索引的创建删除

 - 创建索引:
```sql	
create index emp_ename_index on emp(ename);
```
给emp表的ename字段添加素引，起名：emp_ename_index 

创建  复合索引：

```sql
create index emp_job_sal_index on emp(job,sal);
```
- 删除索引：
```sql
drop index emp_ename_index on emp;
```
将emp表上的emp_ename_index 索引对象删除

### 索引的失效

- 失效的第一种情况

```sql
select from emp where ename like '%T';
```
ename上即使添加了索引，也不会走索引,原因是模糊匹配当中以'%'号开头了

尽量避免模糊查询时以'%'开始,这是一种优化的手段/策略

- 失效的第二种情况

使用`or`有时会失效，如果使用`or`，要求`or`两边的条件字段都要有索引，才会走索引，如果其中一边有一个字段没有索引，那么另一个字段上的索引也会失效  这是不建议使用`or`的原因（建议使用`union`)

- 失效的第三种情况

使用复合索引时没有使用左侧列查找，索引失效

- 失效的第四种情况

在`where`当中**索引列**参加了运算

```sql
select from emp where sal+1  = 800;
```

- 失效的第五种情况

在`where`当中索引列使用了函数 

```sql
select * where lower(ename) = 'smith';
```
- ...

### 索引的分类

- 单一索引:一个字段上添加索引
- 复合索引：两个或多个字段添加索引
- 主键索引：主键添加索引
- 唯一性索引：`unique`约束的字段添加索引 
-  ......
-  注意：唯一性比较弱的字段上添加索引的用处不大

## 视图

view:站在不同的角度去看待同一份数据

### 创建删除视图

- 创建视图对象:
```sql
create view emp_view as select * from emp;
```
- 删除视图对象：
```sql
drop view emp_view;
```
- 注意只有DQL语句才能以view的形式创建:

create view view_name as **这里的语句必须是DQL语句** ；

### 视图的作用

可以面向视图对象进行增删改查，对视图对象的增删改查，会导致
原表被操作（视图的特点：==对视图的操作，会影响到原表数据==）

假设有一条复杂的SQL语句，而这条SQL语句需要在不同的位置上反复使用.
若每一次使用这个SQL语句的时候都需要重新编写，很长，很麻烦.可以把这条复杂的SQL语句以视图对象的形式新建, 在需要编写这条SQL语句的位置直接使用视图对象，可以**简化开发**,并且**利于后期维护**，因为==修改的时候也只需要修改视图对象所映射的SQL语句==.