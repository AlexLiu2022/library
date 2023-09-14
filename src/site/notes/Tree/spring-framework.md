---
{"dg-publish":true,"dg-path":"  spring-framework.md","permalink":"/spring-framework/","tags":["CS/programming-languages/java/java-frameworks/spring"],"created":"2022-08-15T15:43:21.005+08:00","updated":"2023-09-14T17:17:28.454+08:00"}
---


# Introduction

**Spring Framework是Spring生态圈中最基础的项目，是其他项目的根基**

# Architecture

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/architecture-of-spring-framwork.png)

# Core Container

## Core concepts

### IoC

代码书写现状
- 耦合度偏高

解决方案
- 使用对象时，在程序中不要主动使用new产生对象，转换为由外部提供对象
 
**IoC(Inversion of Control)控制反转**

==对象的创建控制权由程序转移到外部，这种思想称为控制反转==

#### Bean 

Spring技术对IoC思想进行了实现:
- Spring提供了一个容器，称为IoC容器，用来充当Ioc思想中的外部
- IoC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IoC容器中统称为Bean

#### DI

Dependency Injection, 依赖注入

在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入

#### Aim

目标：充分解耦

- 使用Ioc容器管理bean

- 在Ioc容器内将有依赖关系的bean进行关系绑定(DI)

最终效果

- 使用对象时不仅可以直接从Ioc容器中获取，并且获取到的bean已经绑定了所有的依赖关系

## Cases

### IoC case

**Analysis of Ideas**

1. 管理什么？(Service与Dao)  // Dao就是Mapper
2. 如何将被管理的对象告知IoC容器？（配置)
3. 被管理的对象交给IoC容器，如何获取到Ioc容器？（接口)
4. Ioc容器得到后，如何从容器中获取bean?（接口方法)
5. 使用Spring导入哪些坐标？（pom.xml)

---

Case in XML Way

1. 导入Springs坐标

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context</artifactId>
	<version>5.2.10.RELEASE</version>
</dependency>
```

2. 定义Spring管理的类（接口）

```java
public interface BookService{
	public void save();
}
```
```java
public class BookServiceImpl implements BookService{
	private BookDao bookDao = new BookDaoImpl();
	public void save(){
		bookDao.save();
	}
}
```
3. 创建Spring配置文件，配置对应类作为Spring管理的bean

```xml
<?xml version="1.0"encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
https://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="bookService"class="com.test.service.impl.BookServiceImpl"></bean>
</beans>
```

4. 初始化Ioc容器(Spring核心容器/Spring容器)，通过容器获取bean

```java
public class App{
	public static void main(String[]args){
	//加载配置文件得到上下文对像，也就是容器对象
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
	//获取资源
		BookService bookService =(BookService)ctx.getBean("bookService");
		bookService.save();
	}
}
```

### DI case

**Analysis of Ideas**

1. 基于IoC管理bean
2. Service中使用new形式创建的Dao对象是否保留？（否）
3. Service中需要的Dao对象如何进入到Service中？（提供方法)
4. Service与Dao间的关系如何描述？（配置)

Case in XML Way

1. 删除使用new的形式创建对象的代码

```java
public class BookServiceImpl implements BookService{
	private BookDao bookDao = new BookDaoImpl();
	public void save(){
		bookDao.save();
	}
}
```

2. 提供依赖对象对应的setter方法
```java
public class BookServiceImpl implements BookService{

	private BookDao bookDao;
	
	public void save(){
		bookDao.save();
	}
	public void setBookDao(BookDao bookDao){
		this.bookDao = bookDao;
	}
}
```

3. 配置service与dao之间的关系

```xml
<?xml version="1.0"encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
https://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="bookService"class="com.test.di.service.impl.BookServiceImpl">
		<property name="bookDao" ref="bookDao"/>
	</bean>
	<bean id="bookDao"class="com.test.di.dao.impl.BookDaoImpl"/>
</beans>
```

- property标签表示配置当前bean的属性
- name属性表示配置哪一个具体的属性
- ref属性表示参照哪一个bean

## Bean

### Config 

**基础配置**

参照上方案例

**别名配置**

- 定义bean的别名，可定义多个
- 使用逗号(，)分号(；)空格()分隔

范例

```xml
<bean id="bookDao"name="dao bookDaoImpl"class="com.test.dao.impl.BookDaoImpl"/>
<bean name="service,bookServiceImpl"class="com.test.service.impl.BookServiceImpl"/>
```

==注意事项==

获取bean无论是通过id还是name获取，如果无法获取到，将抛出异常`NoSuchBeanDefinitionException`

```
NoSuchBeanDefinitionException:No bean named 'bookServiceImpl'available
```

**范围配置**

定义bean的作用范围，可选范围如下

- singleton:单例（默认）
- prototype:非单例

```xml
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl"scope="prototype"/>
```

说明：

bean默认为单例的原因：容器每使用一次bean都新建一个对象，容器会异常庞大，Spring不用来管理这一类bean

适合交给容器进行管理的bean

表现层对象

- 业务层对象

- 数据层对象

- 工具对象

不适合交给容器进行管理的bean

- 封装实体的域对象

### Instantiation

bean实例化

实例化bean的三种方式 (Spring提供第四种)

**构造方法（常用）**
- 提供可访问的构造方法
```java
public class BookDaoImpl implements BookDao{
	public BookDaoImpl(){
		System.out.println("book constructor is running ...")
	}
	public void save(){
		System.out.println("book dao save ...")
	}
}
```
- 配置
	- 与前面相同

无参构造方法如果不存在，将抛出异常`BeanCreationException`


**静态工厂**

- 静态工厂

```java
public class OrderDaoFactory{
	public static OrderDao getorderDao(){
		return new OrderDaoImpl();
	}
}	
```
- 配置

```xml
<bean
	id="orderDao"
	factory-method="getorderDao"
	class="com.test.factory.OrderDaoFactory"
/>
```


**实例工厂**

- 实例工厂

```java
public class UserDaoFactory{
	public UserDao getUserDao(){
		return new UserDaoImpl();
	}
}	
```

- 配置

```java
<bean id=
userDaoFactory
class="com.test.factory.UserDaoFactory"/>

<bean
	id="userDao"
	factory-method="getUserDao"
	factory-bean="userDaoFactory"
/>
```

**FactoryBean**

- FactoryBean
```java
public class UserDaoFactoryBean implements FactoryBean<UserDao>{
	public UserDao getobject()throws Exception{
		return new UserDaoImpl();
	}	
	public Class<?> getobjectType(){
		return UserDao.class;
	}
}	
```

- 配置
```xml
<bean
	id="userDao"
	class="com.test.factory.UserDaoFactoryBean"
/>
```



### Life Circle

#### Control 

方式1:提供生命周期控制方法

```java
public class BookDaoImpl implements BookDao{
	public void save(){
		System.out.println("book dao save...");
	}	
	public void init(){
		System.out.println("book init ...")
	}	
}
	public void destory(){
		System.out.println("book destory ...")
	}
}	
```
- 配置生命周期控制方法

```xml
<bean id="bookDao"
			class="com.test.dao.impl.BookDaoImpl"
			init-method="init"
			destroy-method="destory"
/>
```

---

方式2:实现InitializingBean,DisposableBean接口

```java
public class BookServiceImpl implements BookService,InitializingBean,DisposableBean{
	public void save(){
		System.out.println("book service save ...")
	}	
	public void afterPropertiesset()throws Exception{
		System.out.println("afterPropertiesSet");
	}
	public void destroy()throws Exception{
		System.out.println("destroy");
	}
}
```

---

#### Process

- 初始化容器
	1. 创建对象（内存分配)
	2. 执行构造方法
	3. 执行属性注入(set操作)
	4. 执行bean初始化方法
- 使用bean
	1. 执行业务操作
- 关闭/销毁容器
	1. 执行bean销毁方法

---
bean销毁时机

- 容器关闭前触发bean的销毁

关闭容器方式：
- 手工关闭容器

	- ConfigurableApplicationContext接口close()操作

- 注册关闭钩子，在虚拟机退出前先关闭容器再退出虚拟机

	- ConfigurableApplicationContext接口registerShutdownHook()操作

```java
public class AppForLifeCycle{
	public static void main(String[] args){
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
		ctx.close();
	}	
}
```

## DI

### DI methods

**依赖注入方式**

setter注入

- 引用类型

在bean中定义引用类型属性并提供可访问的set方法
```java
public class BookServiceImpl implements BookService{
	private BookDao bookDao;
	public void setBookDao(BookDao bookDao){
		this.bookDao = bookDao;
	}
}
```
配置中使用property标签ref属性注入引用类型对象
```xml
<bean id="bookService"class="com.test.service.impl.BookServiceImpl">
	<property name="bookDao"ref="bookDao"/>
</bean>
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl"/>
```
- 简单类型

配置中使用property标签value属性注入简单类型数据
```xml
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl">
	<property name="connectionNumber"value="10"/>
</bean>
```


 构造器注入

- 引用类型

在bean中定义引用类型属性并提供可访问的构造方法
```java
public class BookServiceImpl implements BookService{
	private BookDao bookDao;
	public BookServiceImpl(BookDao bookDao){
		this.bookDao = bookDao;
	}
}
```
配置中使用constructor-arg标签ref属性注入引用类型对象
```xml
<bean id="bookService"class="com.test.service.impl.BookServiceImpl">
	<constructor-arg name="bookDao"ref="bookDao"/>
</bean>
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl"/>
```

- 简单类型

配置中使用constructor-arg标签value属性注入简单类型数据
```xml
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl">
	<constructor-arg name="connectionNumber"value="10"/>
</bean>
```

- 参数适配
	- 解决构造器方式注入由于配置中填形参名造成高耦合问题，但同样缺陷很大

配置中使用constructor-arg标签type属性设置按形参类型注入
```xml
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl">
	<constructor-arg type="int"value="10"/>
	<constructor-arg type="java.lang.String"value="mysql"/>
</bean>
```
配置中使用constructor-arg标签index/属性设置按形参位置注入
```xml
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl">
	<constructor-arg index="0"value="10"/>
	<constructor-arg index="1"value="mysql"/>
</bean>
```

---

==依赖注入方式选择==

1. 强制依赖使用构造器进行，==使用setter注入有概率不进行注入导致null对象出现== (构造器未执行直接报错)
2. 可选依赖使用setter注入进行，灵活性强
3. Spring框架倡导使用构造器，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
4. 如果有必要可以两者同时使用，使用构造器注入完成强制依赖的注入，使用sette注入完成可选依赖的注入
5. 实际开发过程中还要根据实际情况分析，如果受控对象没有提供setter方法就必须使用构造器注入
6. ==自己开发的模块推荐使用setter注入==


### Autowire 

依赖自动装配

- Ioc容器根据bean所依赖的资源在容器中自动查找并注入到bean中的过程称为自动装配
- 自动装配方式
	- 按类型（常用）
	- 按名称
	- 按构造方法
	- 不启用自动装配


配置中使用bean标签autowire属性设置自动装配的类型

```xml
<bean id="bookDao"class="com.test.dao.impl.BookDaoImpl"/>
<bean id="bookService"class="com.test.service.impl.BookServiceImpl"autowire="byType"/>
```

**依赖自动装配特征**

- 自动装配用于引用类型依赖注入，不能对简单类型进行操作
- 使用按类型装配时（byType)必须保障容器中相同类型的bean唯一，推荐使用
- 使用按名称装配时（byName)必须保障容器中具有指定名称的bean,因变量名与配置耦合，不推荐使用
- 自动装配优先级低于setter注入与构造器注入，同时出现时自动装配配置失效

### Collection Injection

- 注入数组对象

```xml
<property name="array">
	<array>
		<value>100</value>
		<value>200</value>
		<value>300</value>
	</array>
</property>
```
- 注入List对象（重点)
```xml
<property name="list">
	<list>
		<value>it</value>
		<value>jjj</value>
		<value>bbb</value>
	</list>
</property>
```
- 注入Set对象
```xml
<property name="set">
	<set>
		<value>it</value>
		<value>jjj</value>
		<value>bbb</value>
	</set>
</property>
```
- 注入Map对象（重点）
```xml
<property name="map">
	<map>
		<entry key="country"value="china"/>
		<entry key="province"value="henan"/>
		<entry key="city"value="kaifeng"/>
	</map>
</property>
```
 - 注入Propertiesi对象
```xml
<property name="properties">
	<props>
		<prop key="country">china</prop>
		<prop key="province">henan</prop>
		<prop key="city">kaifeng</prop>
	</props>
</property>
```

## Load properties file 

- 开启context命名空间
```xml
<?xml version="1.0"encoding="UTF-8"?>
<beans xmlns="htatp://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="
	http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```
- 使用context命名空间，加载指定properties文件
```xml
<context:property-placeholder location="jdbc.properties"/>
```
-  使用${)读取加载的属性值
```xml
<property name="username"value="$fidbc.username}"/>
```

---

- 不加载系统属性
```xml
<context:property-placeholder location="jdbc.properties"system-properties-mode="NEVER"/>
```
- 加载多个properties文件
```xml
<context:property-placeholder location="jdbc.properties,msg.properties"/>
```
加载所有properties文件
```xml
<context:property-placeholder location="*.properties"/>
加载properties文件标准格式
```xml
<context:property-placeholder location="classpath:*.properties"/>
```
从类路径或jar包中搜索并加载properties文件
```xml
<context:property-placeholder location="classpath*:*properties"/>
```

## Container

### Create

创建容器
- 方式一：类路径加载配置文件
```java
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
```

- 方式二：文件路径加载配置文件
```java
ApplicationContext ctx = new FileSystemXmlApplicationContext("D:\\applicationContext.xml");
```
- 加载多个配置文件
```java
ApplicationContext ctx = new ClassPathXmlApplicationContext("bean1.xml","bean2.xml");
```

--- 

获取bean
- 方式一：使用bean名称获取
```java
BookDao bookDao = (BookDao) ctx.getBean("bookDao");
```
- 方式二：使用bean名称获取并指定类型
```java
BookDao bookDao = ctx.getBean("bookDao",BookDao.class);
```
- 方式三：使用bean类型获取
```java
BookDao bookDao = ctx.getBean(BookDao.class);
```

### Hierarchy

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/hierarchy-fo-IoC-container.png)


BeanFactory初始化

- 类路径加载配置文件
```java
Resource resources = new ClassPathResource("applicationContext.xml");
BeanFactory bf =  new XmlBeanFactory(resources);
BookDao bookDao bf.getBean("bookDao",BookDao.class);
bookDao.save();
```
- BeanFactory创建完毕后，所有的bean均为延迟加载

---
### Related details

- BeanFactory是IoC容器的顶层接口，初始化BeanFactoryi对象时，加载的bean延迟加载
- ApplicationContext接口是Spring容器的核心接口，初始化时bean立即加载
- ApplicationContext接口提供基础的bean操作相关方法，通过其他接口扩展其功能

ApplicationContext接口常用初始化类
- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext

## Annotation

注解开发定义bean

- 使用@Component定义bean
```java
@Component("bookDao")
public class BookDaoImpl implements BookDao{
}
@Component
public class BookServiceImpl implements BookService{
}
```
- 核心配置文件中通过组件扫描加载bean
```xml
<context:component-scan base-package="com.package_name"/>
```

Spring提供@Component注解的三个衍生注解

- @Controller:用于表现层bean定义
- @Service:用于业务层bean定义
- @Repository:用于数据层bean定义

```java
@Repository("bookDao")
public class BookDaoImpl implements BookDao{
}
@Service
public class BookServiceImpl implements BookService{
}
```

---

**纯注解开发**

- Spring3.0开启了纯注解开发模式，使用Java类替代配置文件，开启了Spring快速开发赛道
- Java类代替Spring核心配置文件

```java
@Configuration
@ComponentScan("com.package_name")
public class SpringConfig{
}
```

- @Configuration注解用于设定当前类为配置类
- @ComponentScan注解用于设定扫描路径，此注解只能添加一次，多个数据用数组格式
 ```java
@ComponentScan({"com.test.service","com.test.dao"})
```

读Spring核心配置文件初始化容器对象切换为读取Java配置类初始化容器对象
```java
//加载配置文件初始化容器
ApplicationContext ctx  = new ClassPathXmlApplicationContext("applicationContext.xml");
//加载配置类初始化容器
ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
```


**bean作用范围**

- 使用@Scope定义bean作用范围
```java
@Repository
@Scope("singleton")
public class BookDaoImpl implements BookDao{
}
```

**bean生命周期**

- 使用@PostConstruct、@PreDestroy定义bean生命周期
```java
@Repository
@Scope("singleton")
public class BookDaoImpl implements BookDao{
	public BookDaoImpl(){
		System.out.println("book dao constructor ...")
	}
	@PostConstruct
	public void init(){
		System.out.println("book init ...")
	}
	@PreDestroy
	public void destroy(){
		System.out.println("book destory ...")
	}
}
```

**依赖注入**

- 使用@Autowired注解开启自动装配模式（按类型)
```java
@Service
public class BookServiceImpl implements BookService{
	@Autowired
	private BookDao bookDao;
	
	//public void setBookDao(BookDao bookDao){
	//	this.bookDao bookDao;
	//}

	public void save(){
		System.out.println("book service save...");
		bookDao.save();
	}
}
```

- 注意：==自动装配基于反射==设计创建对象并暴力反射对应属性为私有属性初始化数据，因此==无需提供setter方法==
- 注意：自动装配建议使用无参构造方法创建对象（默认），如果不提供对应构造方法，请提供唯一的构造方法


- 使用@Qualifier注解开启指定名称装配bean
```java
@Service
public class BookServiceImpl implements BookService{
	@Autowired
	@Qualifier("bookDao")
	private BookDao bookDao;
}
```
注意：@Qualifier注解无法单独使用，必须配合@Autowired注解使用


- 使用@Value实现**简单类型注入**
```java
@Repository("bookDao")
public class BookDaoImpl implements BookDao{
	@Va1ue("100")
	private String connectionNum;
}
```


**加载properties文件**

- 使用@PropertySource注解加载properties文件
```java
@Configuration
@ComponentScan("com.test")
@PropertySource("classpath:jdbc.properties")
public class SpringConfig
```
注意：路径仅支持单一文件配置，多文件使用数组格式配置，==不允许使用通配符`*`==

**第三方Bean管理**

- 使用@Bean配置第三方bean
```java
@Configuration
public class SpringConfig{
	@Bean
	public DataSource dataSource(){
		DruidDataSource ds = new DruidDataSource()
		ds.setDriverclassName("com.mysql.jdbc.Drive
		ds.setUrl("jdbc:mysql://localhost:3306/spri
		ds.setusername("root");
		ds.setPassword("root");
		return ds;
	}	
}
```

**将独立的配置类加入核心配置**

- 方式一：导入式
```java
public class JdbcConfig{
	@Bean
	public DataSource dataSource(){
		DruidDataSource ds new DruidDataSource();
		//相关配置
		return ds;
	}
} 
```
- 使用@Import注解手动加入配置类到核心配置，此注解只能添加一次，多个数据用数组格式
```java
@Configuration
@Import(JdbcConfig.class)
	public class SpringConfig{
}
```

- 方式二：扫描式

```java
@Configuration
public class JdbcConfig{
	@Bean
	public DataSource dataSource(){
		DruidDataSource ds = new DruidDataSource();
		//相关配置
		return ds;
	}
}
```
- 使用@ComponentScan注解扫描配置类所在的包，加载对应的配置类信息
```java
@Configuration
@ComponentScan({"com.test.config","com.test.service","com.test.dao"})
public class SpringConfig{
}
```

**第三方bean依赖注入**

- 简单类型依赖注入
```java
public class JdbcConfig
	@Value("com.mysq1.jdbc.Driver")
	private String driver;
	@Value("jdbc:mysq1://localhost:3306/spring_db")
	private string url;
	@Value("root")
	private String userName;
	@Value("root")
	private String password;
	@Bean
	public DataSource dataSource(){
		DruidDataSource ds new DruidDataSource();
		ds.setDriverclassName(driver);
		ds.setUrl(url);
		ds.setUsername(userName);
		ds.setPassword(userName);
		return ds;
	}
}
```

- 引用类型依赖注入
```java
@Bean
public DataSource dataSource(BookService bookService){
	System.out.println(bookService);
	DruidDataSource ds new DruidDataSource();
	//属性设置
	return ds;
}
```
引用类型注入只需要为bean定义方法设置形参即可，容器会根据类型自动装配对象


## Summary

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/spring-framwork-IoC-summary.png)

---

# Integration

## Mybatis

Spring整合JUnit

- SqlSessionFactoryBean替代原来sqlSession相关配置
	- 用set语句配置具体信息
	- 数据源对象通过注入形式加入

```java
@Bean
public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
	SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
	ssfb.setTypeAliasesPackage("com.test.domain");
	ssfb.setDataSource(dataSource);
	return ssfb;
}
```

-  MapperScannerConfigurer替代原来Mapper映射配置

```java
@Bean
public MapperScannerConfigurer mapperScannerConfigurer(){
	MapperScannerConfigurer msc = new MapperScannerConfigurer();
	msc.setBasePackage("com.test.dao");
	return msc;
}
```

## JUnit

Spring整合JUnit

- 使用Spring整合Junit专用的类加载器
```java
@Runwith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest{
	@Autowired
	private BookServicebookService;
	
	@Test
	public void testSave(){
		bookService.save();
	}
}
```


# AOP

## Introduction

AOP(Aspect Oriented Programming) 面向切面编程，一种编程范式，指导开发者如何组织程序结构
- 类似：OOP(Object Oriented Programming) 面向对象编程，为一种编程思想

作用：在不惊动原始设计的基础上为其进行功能增强

Spring理念：无入侵式/无侵入式 

## Core concepts

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-AOP.png)


- 连接点(JoinPoint):程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等
	- 在SpringAOP中，理解为方法的执行
- 切入点(Pointcut):匹配连接点的式子
	- 在SpringAOP中，一个切入点可以只描述一个具体方法，也可以匹配多个方法
		- 一个具体方法：com.test.dao包下的BookDao接口中的无形参无返回值的save方法
		- 匹配多个方法：所有的save方法，所有的get开头的方法，所有以Dao结尾的接口中的任意方法，所有带有一个参数的方法
- 通知(Advice):在切入点处执行的操作，也就是共性功能
	- 在SpringAOP中，功能最终以方法的形式呈现
- 通知类：定义通知的类
- 切面(Aspect):描述通知与切入点的对应关系

---

- 目标对象（Target)：原始功能去掉共性功能对应的类产生的对象，这种对象是无法直接完成最终工作的
- 代理(Poxy):目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现

## Get Started

**Analysis of Ideas**

案例设定：测定接口执行效率
简化设定：在接口执行前输出当前系统时间
开发模式：XML or Annotation

思路分析：
1. 导入坐标(pom.xml)
2. 制作连接点方法（原始操作，Dao接口与实现类）
3.  制作共性功能（通知类与通知)
4. 定义切入点
5. 绑定切入点与通知关系（切面)

---

AOP入门案例（Annotation type）
1. 导入aop相关坐标
```xml
<dependency>
	<groupId>org.aspectj</groupId>
	<artifactId>aspectjweaver</artifactId>
	<version>1.9.4</version>
</dependency>
```
说明：spring-aop坐标依赖spring-context坐标

org.springframework:spring-context:5.2.10.RELEASE
- org.springframework:spring-aop:5.2.10.RELEASE
- org.springframework:spring-beans:5.2.10.RELEASE
- org.springframework:spring-core:5.2.10.RELEASE
- org.springframework:spring-expression:5.2.10.RELEASE

org.aspectj:aspectjweaver:1.9.4

2. 定义dao接口与实现类
```java
public interface BookDao{
	public void save();
	public void update();
}
```
```java
@Repository
public class BookDaoImpl implements BookDao{
	public void save(){
		System.out.println(System.currentTimeMillis());
		System.out.println("book dao save...");
	}

	public void update(){
		System.out.println("book dao update...");
	}	
}
```

3. 定义通知类，制作通知
```java
public class MyAdvice{
	public void before(){
		System.out.printIn(System.currentTimeMillis());
	}
}
```

4. 定义切入点
```java
public class MyAdvice{
	@Pointcut("execution(void com.test.dao.BookDao.update())")
	private void pt(){}
```
说明：切入点定义依托一个不具有实际意义的方法进行，即无参数，无返回值，方法体无实际逻辑

5. 绑定切入点与通知关系，并指定通知添加到原始连接点的具体执行位置

```java
public class MyAdvice{
	@Pointcut("execution(void com.test.dao.BookDao.update())")
	private void pt(){}
	
	@Before("pt()")
	public void before(){
		System.out.printIn(System.currentTimeMillis());
	}
}
```
6. 定义通知类受Spring容器管理，并定义当前类为切面类
```java
@Component
@Aspect
public class MyAdvice{
	@Pointcut("execution(void com.test.dao.BookDao.update())")
	private void pt(){}
	
	@Before("pt()")
	public void before(){
		System.out.println(System.currentTimeMillis());
	}
}
```

7. 开启Spring对AOP注解驱动支持
```java
@Configuration
@Componentscan("com.test")
@EnableAspectJAutoProxy
public class SpringConfig{
}
```

## Process

AOP工作流程

1. Spring容器启动
2. 读取所有切面配置中的切入点

```java
@Component
@Aspect
public class MyAdvice{
	@Pointcut("execution(void com.test.dao.BookDao.save())")
	private void ptx(){}
	
	@Pointcut("execution(void com.test.dao.BookDao.update())")
	private void pt(){}

	@Before("pt()")
	public void method(){
		System.out.println(System.currentTimeMillis());
	}
}
```
3. 初始化bean,判定bean对应的类中的方法是否匹配到任意切入点
- 匹配失败，创建对象
- 匹配成功，创建原始对象（目标对象）的代理对象
4. 获取bean执行方法
- 获取bean,调用方法并执行，完成操作
- 获取的bean是代理对象时，根据代理对象的运行模式运行原始方法与增强的内容，完成操作

---

## Pointcut Expression

AOP切入点表达式

- 切入点：要进行增强的方法
- 切入点表达式：要进行增强的方法的描述方式
```java
package com.test.dao;
public interface BookDao{
	public void update();
}
```
```java
public class BookDaoImpl implements BookDao{
	public void update(){
		System.out.println("book dao update ...")
	}
}
```
 描述方式一：
- 执行com.test.dao包下的BookDao接口中的无参数update方法

- `execution(void com.test.dao.BookDao.update())`

 描述方式二：
- 执行com.test.dao.impl包下的BookDaoImpl类中的无参数update方法

- `execution(void com.test.dao.impl.BookDaoImpl.update())`

### Standard Format

 
切入点表达式标准格式：动作关键字（访问修饰符 返回值 包名.类/接口名.方法名（参数）异常名)
```
execution (public User com.test.service.UserService.findById (int))
```
- 动作关键字：

描述切入点的行为动作，例如execution表示执行到指定切入点

- 访问修饰符：public,private等 可以省略
- 返回值
- 包名
- 类/接口名
- 方法名
- 参数
- 异常名：方法定义中抛出指定异常，可以省略

### Wildcard

可以使用通配符描述切入点，快速描述
- `*`:单个独立的任意符号，可以独立出现，也可以作为前缀或者后缀的匹配符出现

`execution (public com.test.*.UserService.find*(*)`

匹配com.test包下的任意包中的UserService类或接口中所有find开头的带有一个参数的方法
- `..`:多个连续的任意符号，可以独立出现，常用于简化包名与参数的书写

`execution (public User com..UserService.findById (..)`

匹配com包下的任意包中的UserService类或接口中所有名称为findByld的方法

- `+`:专用于匹配子类类型

`execution(**..*Service+.*(..))`


### Tips

**书写技巧**

- 所有代码按照标准规范开发，否则以下技巧全部失效
- 描述切入点通常描述接口，而不描述实现类(降低耦合)
- 访问控制修饰符针对接口开发均采用oublic描述（可省略访问控制修饰符描述）
- 返回值类型对于增删改类使用精准类型加速匹配，对于查询类使用`*`通配快速描述
- 包名书写尽量不使用匹配，效率过低，常用`*`做单个包描述匹配，或精准匹配
- 接口名/类名书写名称与模块相关的采用`*`匹配，例如U serService书写成`*`Service,绑定业务层接口名
- 方法名书写以动词进行精准匹配，名词采用`*`匹配，例如getByld书写成getBy`*`,==selectAll书写成selectAll==
- 参数规则较为复杂，根据业务方法灵活调整
- 通常不使用异常作为匹配规则 

## Advice Types

AOP通知描述了抽取的共性功能，根据共性功能抽取的位置不同，最终运行代码时要将其加入到合理的位置
A0P通知共分为5种类型

- 前置通知

名称：@Before
类型：方法注解
位置：通知方法定义上方
作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前运行
范例
```java
@Before("pt()")
public void before(){
	System.out.println("before advice ...")
}
```
相关属性：vlue（默认)：切入点方法名，格式为类名.方法名()

- 后置通知

名称：@After
类型:方法注解
位置：通知方法定义上方
作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法后运行
范例：
```java
@After("pt()")
public void after(){
System.out.println("after advice...");
}
```
相关属性：value（默认)：切入点方法名，格式为类名.方法名()

- ==环绕通知==

名称：@Around
类型：方法注解
位置：通知方法定义上方
作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前后运行
范例：
```java
@Around("pt()")
public Object around(ProceedingJoinPoint pjp)throws Throwable{
	System.out.println("around before advice...");
	Object ret = pjp.proceed();
	System.out.println("around after advice ...")
	return ret;
```

---

**@Around注意事项**
1. 环绕通知必须依赖形参ProceedingJoinPoint才能实现对原始方法的调用，进而实现原始方法调用前后同时添加通知
2. 通知中如果未使用ProceedingJoinPoint对原始方法进行调用将跳过原始方法的执行
3. ==对原始方法的调用可以不接收返回值，通知方法设置成void即可，如果接收返回值，必须设定为Objecta类型==
4. 原始方法的返回值如果是void类型，通知方法的返回值类型可以设置成void,也可以设置成Object
5. 由于无法预知原始方法运行后是否会抛出异常，因此环绕通知方法必须抛出Throwable对象

```java
@Around("pt()")
public Object around(ProceedingJoinPoint pjp)throws Throwable{
	System.out.println("around before advice ...")
	object ret = pjp.proceed();
	System.out.println("around after advice ...")
	return ret;
}
```
- 返回后通知
- 抛出异常后通知

## Case 1

案例: 测量业务层接口执行效率
需求: 任意业务层接口执行均可显示其执行效率（执行时长）
分析:
1. 业务功能：业务层接口执行前后分别记录时间，求差值得到执行效率
2. 通知类型选择前后均可以增强的类型一环绕通知

**补充说明**

当前测试的接口执行效率仅仅是一个理论值，并不是一次完整的执行过程

测量业务层接口执行效率（万次核心代码执行效率）

```java
@Around("ProjectAdvice.servicept()")
public void runSpeed(ProceedingJoinPoint pjp)throws Throwable{
	long start = System.currentTimeMillis();
	for(inti=0;i<10000;i++){
		pjp.proceed();
	}
	long end = System.currentTimeMillis();
	System.out.printIn("业务层接口万次执行时间："+(end-start)+"ms");
}
```
 
```java
@Around("ProjectAdvice.servicept()")
public void runSpeed(ProceedingJoinPoint pjp)throws Throwable{
//获取执行签名信息
	Signature signature = pjp.getsignature();
//通过签名获取执行类型（接口名）
	String className signature.getDeclaringTypeName();
//通过签名获取执行操作名称（方法名）
	String methodName signature.getName();
	long start System.currentTimeMillis();
	for (int i=0;i<10000;i++){
		pjp.proceed();
	}
	long end = System.currentTimeMillis();
	System.out.println("万次执行："+className+-"."+methodName+"--->"+(end-start)+"ms");
}
```


## Get Data

**获取切入点方法的参数**

- JoinPoint对象描述了连接点方法的运行状态，可以获取到原始方法的调用参数
```java
@Before("pt()")
public void before(JoinPoint jp){
	object[]args jp.getArgs();
	System.out.println(Arrays.tostring(args));
}
```

- ProceedingJointPoint是JoinPoint的子类

```java
@Around("pt()")
public Object around(ProceedingJoinPoint pjp)throws Throwable{
	object[]args pjp.getArgs();
	System.out.println(Arrays.toString(args));
	Object ret pjp.proceed();
	return ret;
}
```


- JoinPoint:适用于前置、后置、返回后、抛出异常后通知
- ProceedingJointPoint:适用于环绕通知
- ==如在参数中出现必须为第一个参数==

**获取切入点方法返回值**

 

- 抛出异常后通知可以获取切入点方法中出现的异常信息，使用形参可以接收对应的异常对象
```java
@AfterReturning(value = "pt()",returning = "ret")
public void afterReturning(String ret){
	System.out.println("afterReturning advice..."+ret);
}
```
- 环绕通知中可以手工书写对原始方法的调用，得到的结果即为原始方法的返回值
```java
@Around("pt()")
public Object around(ProceedingJoinPoint pjp)throws Throwable{
	object ret pjp.proceed();
	return ret;
}
```



**获取切入点方法运行异常信息**

- 抛出异常后通知
- 环绕通知

---

- 抛出异常后通知可以获取切入点方法中出现的异常信息，使用形参可以接收对应的异常对象
```java
@AfterThrowing(value "pt()",throwing "t")
public void afterThrowing(Throwable t){
	System.out.println("afterThrowing advice ..." + t);
}
```

- 抛出异常后通知可以获取切入点方法运行的异常信息，使用形参可以接收运行时抛出的异常对象
```java
@Around("pt()")
public Object around(ProceedingJoinPoint pjp){
	Object ret=  null;
	try{
		ret =  pjp.proceed();
	}catch (Throwable t){
		t.printStackTrace();
	}
	return ret;
}
```

## Case 2

案例 百度网盘分享链接输入密码数据错误兼容性处理

需求：对百度网盘分享链接输入密码时尾部多输入的空格做兼容处理

分析：
1. 在业务方法执行之前对所有的输入参数进行格式处理---trim()
2. 使用处理后的参数调用原始方法---环绕通知中存在对原始方法的调用

```java
@Around("DataAdvice.servicePt()")
public object trimString(ProceedingJoinPoint pjp)throws Throwable{
	object[]args pjp.getArgs();
	//对原始参数的每一个参数进行操作
	for (int i 0;i<args.length;i++){
		//如果是字符串数据
		if(args[i].getclass().equals(String.class)){
			//取出数据，trim()操作后，更新数据
			args[i] = ar gs[i].toString().trim();
		}
	}
	return pjp.proceed(args);
}
```

# Transaction

## Introduction

Spring事务简介

- 事务作用：

在数据层保障一系列的数据库操作同成功同失败
- Spring事务作用：

在数据层或业务层保障一系列的数据库操作同成功同失败

---

```java
public interface PlatformTransactionManager{
	void commit(TransactionStatus status)throws TransactionException;
	void rollback(TransactionStatus status)throws TransactionException;
}
```
```java
public class DataSourceTransactionManager{
 ...
}
```

## Case 1  

 案例 ：模拟银行账户间转账业务
 
需求：实现任意两个账户间转账操作
需求微缩：A账户减钱，B账户加钱

分析：
1. 数据层提供基础操作，指定账户减钱(outMoney),指定账户加钱(inMoney)
2. 业务层提供转账操作（transfer),调用减钱与加钱的操作
3. 提供2个账号和操作金额执行转账操作
4. 基于Spring  整合MyBatis环境搭建上述操作

结果分析：
1. 程序正常执行时，账户金额A减B加，没有问题
2.  程序出现异常后，转账失败，但是异常之前操作成功，异常之后操作失败，整体业务失败

---

1. 在业务层接口上添加Spring事务管理
```java
public interface AccountService{
	@Transactional
	public void transfer(String out,String in Double money);
}
```
注意事项
 
Spring注解式事务通常添加在业务层接口中而不会添加到业务层实现类中，降低耦合
注解式事务可以添加到业务方法上表示当前方法开启事务，==也可以添加到接口上表示当前接口所有方法开启事务==

2. 设置事务管理器
```java
@Bean
public PlatformTransactionManager transactionManager(DataSource dataSource){
	DataSourceTransactionManager ptm = new DataSourceTransactionManager();
	ptm.setDataSource(dataSource);
	return ptm;
}
```
注意事项

事务管理器要根据实现技术进行选择
MyBatis框架使用JDBC事务

3. 开启注解式事务驱动
```java
@Configuration
@ComponentScan("com.test")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
@EnableTransactionManagement
public class SpringConfig{
}
```

## Roles

**Spring事务角色**

- 事务角色
	- 事务管理员：发起事务方，在Spring中通常指代业务层开启事务的方法
	- 事务协调员：加入事务方，在Spring中通常指代数据层方法，也可以是业务层方法

## Config


---

| 属性                   | 作用                       | 示例                                     |
| ---------------------- | -------------------------- | ---------------------------------------- |
| readonly               | 设置是否为只读事务         | readOnly=true只读事务                    |
| timeout                | 设置事务超时时间           | timeout=-1（永不超时)                    |
| ==rollbackFor==            | 设置事务回滚异常(class)    | rollbackFor ={NullPointException.class}  |
| rollbackForClassName   | 设置事务回滚异常(String)   | 同上格式为字符串                         |
| noRollbackFor          | 设置事务不回滚异常（class) | noRollbackFor={NullPointException.class} |
| noRollbackForClassName | 设置事务不回滚异常(String) | 同上格式为字符串                         |
| propagation            | 设置事务传播行为           | ...                                      |


## Case 2

案例 转账业务追加日志
需求：实现任意两个账户间转账操作，并对每次转账操作在数据库进行留痕
需求微缩：A账户减钱，B账户加钱，数据库记录日志

 分析：
1. 基于转账操作案例添加日志模块，实现数据库中记录日志
2. 业务层转账操作(transfer),调用减钱、加钱与记录日志功能

实现效果预期

- 无论转账操作是否成功，均进行转账操作的日志留痕

存在的问题

- 日志的记录与转账操作隶属同一个事务，同成功同失败

---

**事务传播行为**：事务协调员对事务管理员所携带事务的处理态度

1. 在业务层接口上添加Spring事务，设置事务传播行为REQUIRES_.NEW(需要新事务)

```java
@Service
public class LogServiceImpl implements LogService{
	@Autowired
	private LogDao logDao;
	@Transactional(propagation =Propagation.REQUIRES_NEW)
	public void log(String out,String in,Double money ){
		logDao.log("转账操作由"+out+"到"+in+",金额："+money);
	}
}
```
---

**propagation**

---

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/transaction-propagation-in-spring-framwork.png)

---

# SpringMVC

Details see this page: [[Tree/spring-MVC\|spring-MVC]]