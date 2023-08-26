---
{"dg-publish":true,"permalink":"/tree/spring-boot/","tags":["CS/programming-languages/java/java-frameworks/spring"],"created":"2022-08-17T18:19:35.391+08:00","updated":"2023-08-27T03:50:43.937+08:00"}
---


# Get Started

1. 创建新模块，选择Spring初始化，并配置模块相关基础信息
2.  选择当前模块需要使用的技术集
3. 开发控制器类
```java
@RestController
@RequestMapping("/books")
public class BookController{
	@GetMapping("/{id}")
	public String getById(@PathVariable Integer id){
		System.out.println("id =="id);
		return "hello,spring boot!"
	}
}
```
4. 运行自动生成的Application类

---
最简SpringBoot程序所包含的基础文件
- pom.xml文件
- Application类

注意：

**基于idea开发Spring Boot程序需要确保联网且能够加载到程序框架结构**

---

SpringBoot项目快速启动
 
1. 对SpringBoot项目打包（执行Maven:构建指令package)
2. 执行启动指令
```shell
java -jar springboot.jar
```

==注意事项==

jar支持命令行启动需要依赖maven插件支持，打包时要确认是否具有SpringBoot对应的maven插件
```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>
```

# Introduction

SpringBoot概述

SpringBoot是由Pivotal团队提供的全新框架，其设计目的是用来简化Spring应用的初始搭建以及开发过程

---

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-spring-and-springboot.png)


---

Spring程序缺点
- 配置繁琐
- 依赖设置繁琐

SpringBoot程序优点
- 自动配置
- 起步依赖（简化依赖配置）
- 辅助功能（内置服务器，…)

---


**SpringBoot起步依赖**

starter
- SpringBoot中常见项目名称，定义了当前项目使用的所有项目坐标，以达到减少依赖配置的目的

parent
- 所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的

- spring-boot-starter-parent（2.5.)与spring-boot-starter-parent（2.4.6)共计57处坐标版本不同

实际开发
- 使用任意坐标时，仅书写GAV中的G和A,V由SpringBoot提供
- 如发生坐标错误，再指定version(要小心版本冲突)

---

**SpringBoot程序启动**

启动方式
```java
@SpringBootApplication
public class Springboote1QuickstartApplication {
	public static void main(String[] args){
		SpringApplication.run(Springboot01QuickstartApplication.class,args);
	}
}
```

==SpringBoot在创建项目时，采用jar的打包方式==

SpringBoot的引导类是项目的入口，运行main方法就可以启动项目


**使用maven依赖管理变更起步依赖项**
```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
<!--web起步依赖环境中，排除Tomcat,起步依赖-->
		<exclusions>
			<exclusion>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-tomcat</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
	<!--添加jetty起步依赖，版本由SpringBoot的starter控制-->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-jetty</artifactId>
	</dependency>
</dependencies>
```
Jetty比Tomcat更轻量级，可扩展性更强（相较于Tomcat),谷歌应用引擎(GAE)已经全面切换为Jetty

# Config

## format

**配置格式**

- 需求：修改服务器端口
```
http://localhost:8080/books/1
                  ||
                  ||
http://localhost/books/1
```
---
SpringBoot提供了多种属性配置方式

- application.properties
```properties
server.port=80
```
- application.yml
```yml
server:
	port: 81
```
- application.yaml
```yaml
server:
	port: 82
```

---

**springBoot配置文件加载顺优先级**

- application.properties >application.yml> application.yaml

==注意事项==

SpringBoot核心配置文件名为application
SpringBoot内置属性过多，且所有属性集中在一起修改，在使用时，通过提示键+关键字修改属性


通常采用[[Tree/YAML\|YAML]]文件.yml扩展名，即application.yml来配置

---

**YAML数据读取方式**

1. 使用`@Value`读取单个数据，属性名引用方式：${一级属性名.二级属性名...}
```java
@RestController
@RequestMapping("/books")
public class BookController{

	@Value("${lesson}")
	private String lessonName;

	@Value("${server.port}")
	private int port;

	@Value("${enterprise.subject[1]}")
	private String[] subject_01;
}
```

2. 封装全部数据到Environment对象

```java
@RestController
@RequestMapping("/books")
public class BookController{
	@Autowired
	private Environment env;
	@GetMapping("/{id}")
	public String getById(@PathVariable Integer id){
		System.out.println(env.getProperty("lesson"));
		System.out.println(env.getproperty("enterprise.name"));
		System.out.println(env.getProperty("enterprise.subject[]"));
		return "hello spring boot!";
	}
}
```

3. 自定义对象封装指定数据

封装

```java
@Component
@ConfigurationProperties(prefix = "enterprise")
public class Enterprise {
	private String name;
	private Integer age;
	private String[] subject;
}
```
使用
```java
@RestController
@RequestMapping("/books")
public class BookController{
	@Autowired
	private Enterprise enterprise;
}
```
---

**自定义对象封装数据警告解决方案**
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-configuration-processor</artifactId>
	<optional>true</optional>
</dependency>
```

## Profile

**YAML多环境启动**

```yaml
# 过时格式
spring:
	profiles:
		active: pro
---
spring:
	profiles: pro
	
	server:
		port: 80		
---
spring:
	profiles: dev
	
server:
	port: 81
---
spring:
	profiles: test
	
server:
	port: 82
```
---
```yaml
# 推荐格式
spring:
	profiles:
		active: pro
---
server:
	port: 80
	
spring:
	config:
		activate:
			on-profile: pro
```

**properties文件多环境启动**

- 主启动配置文件application.properties
`spring.profiles.active=pro`

- 环境分类配置文件application-pro.properties
`server.port=80`
- 环境分类配置文件application-dev.properties
server.port=81

- 环境分类配置文件application-test.properties
`server.port=82`

---

**带参数启动SpringBoot**

```shell
java -jar springboot.jar --spring.profiles.active=test
```
```shell
java -jar springboot.jar --server.port=88
```
```shell
java -jar springboot.jar --server.port=88 --spring.profiles.active=test
```

---

参数加载优先级顺序参见[Spring官网的描述](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)

---

## Category

**配置文件分类**

SpringBoot中4级配置文件
- 1级：file:config/application.yml【最高】
- 2级：file:application.yml
- 3级：classpath:config/application.yml
- 4级：classpath:application.yml【最低】

作用：
- 1级与2级留做系统打包后设置通用属性
- 3级与4级用于系统开发阶段设置通用属性

# Integration

## JUnit

整合JUnit
SpringBoot整合JUnit

```java
@SpringBootTest
class Springboot_JunitApplicationTests{ //默认和自己项目名相同
	@Autowired
	private BookService bookService;
	@Test
	public void testSave(){
		bookService.save();
	}
}
```
---
名称：**@SpringBootTest**
类型：测试类注解
位置：测试类定义上方
作用：设置JUnit加载的SpringBoot启动类
范例：
```java
@SpringBootTest(classes Springboot_JunitApplication.class)
class Springboot07JunitApplicationTests {
}
```
相关属性
- classes:设置SpringBoot启动类

==注意事项==
**如果测试类在SpringBoot启动类的包或子包中，可以省略启动类的设置，也就是省略classes的设定**


## SSM

基于SpringBoot实现SSM整合

- SpringBoot整合Spring（不存在)
- SpringBoot整合SpringMVC(不存在)
- SpringBoot整合MyBatis(主要)

---

1. 创建新模块，选择Spring初始化，并配置模块相关基础信息
2. 选择当前模块需要使用的技术集（MyBatis、MySQL)
3. 设置数据源参数
```yaml
spring:
	datasource:
		type: com.alibaba.druid.pool.DruidDataSource
		driver-class-name: com.mysql.cj.jdbc.Driver
		url: jdbc:mysq1://localhost:3306/ssm_db
		username: root
		password: root
```
---
==注意事项==
SpringBoot版本低于2.4.3（不含），Mysql驱动版本大于8.0时，需要在ur1连接串中配置时区
```
jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
```
或在MySQL数据库端配置时区解决此问题

4. 定义数据层接口与映射配置
```
@Mapper
public interface UserDao{
	@Select("select * from user")
	public List<User>getAll();
}
```
5. 测试类中注入dao接口，测试功能
```java
@SpringBootTest
class Springboot_MybatisApplicationTests{
	@Autowired
	private BookDao bookDao;
	@Test
	public void testGetById(){
		Bookbook bookDao.getById(1);
		System.out.println(book);
	}
}
```

### Case

基于SpringBoot的SSM整合案例

1. pom.xml

配置起步依赖，必要的资源坐标(druid)

2. application.yml

设置数据源、端口等

3. 配置类

全部删除

4. dao

设置@Mapper

5. 测试类
6. 页面

==放置在resources目录下的static目录中==