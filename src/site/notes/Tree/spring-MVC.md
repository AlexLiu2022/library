---
{"dg-publish":true,"dg-path":"  spring-MVC.md","permalink":"/spring-mvc/","tags":["CS/web/framworks","CS/programming-languages/java/java-frameworks/spring/spring-framework"],"created":"2022-08-16T20:25:27.151+08:00","updated":"2023-09-14T17:17:29.275+08:00"}
---


# Introduction

SpringMVC是一种基于Java实现[[Tree/MVC-pattern\|MVC]]模型的轻量级Web框架,用于进行表现层功能开发

优点:
- 使用简单，开发便捷（相比于[[Tree/Servlet\|Servlet]])

## Get Started

1. 使用SpringMVC技术需要先导入SpringMVC坐标与Servlet坐标
```xml
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>3.1.0</version>
	<scope>provided</scope>
</dependency>
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-webmvc</artifactId>
	<version>5.2.10.RELEASE</version>
</dependency>
```

2. 创建SpringMVC控制器类（等同于Servlet功能）
```java
@Controller
public class UserController{
	@RequestMapping("/save")
	@ResponseBody
	public String save(){
		System.out.printin("user save ...");
		return "{'info':'springmvc'}";
	}
}
```

3. 初始化SpringMVC环境（同Spring环境），设定SpringMVC加载对应的bean

```java
@Configuration
@ComponentScan("com.test.controller")
public class SpringMvcConfig{
}
```

4. 初始化Servlet容器，加载SpringMVC环境，并设置SpringMVC技术处理的请求
```java
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer{
	@Override
	protected WebApplicationContext createServletApplicationContext(){
		AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
		ctx.register(SpringMvcConfig.class);
		return ctx;
	}
	@Override
	protected String[] getServletMappings(){
		return new String[]{"/"};
	}
	@Override
	protected WebApplicationContext createRootApplicationContext(){
		return null;
	}
}
```

---
- 名称：@RequestMapping
- 类型：方法注解
- 位置：SpringMVC控制器方法定义上方
- 作用：设置当前控制器方法请求访问路径

范例：
```java
@RequestMapping("/save")
public void save(){
	System.out.println("user save ...");
}
```
相关属性
- value(默认)：请求访问路径

---
- 名称：@ResponseBody
- 类型：方法注解
- 位置：SpringMVC控制器方法定义上方
- 作用：==设置当前控制器返回值作为响应体==
范例：

```java
@RequestMapping("/save")
@ResponseBody
public String save(){
	System.out.println("user save ...")
	return "{'info':'springmvc'}";
}
```

---

SpringMVC 入门程序开发总结（1+N)
- 一次性工作
	- 创建工程，设置服务器，加载工程
	- 导入坐标
	- 创建web容器启动类，加载SpringMVC配置，并设置SpringMVC请求拦截路径
	- SpringMVC核心配置类（设置配置类，扫描controller包，加载Controller控制器bean)
- 多次工作

	- 定义处理请求的控制器类
	- 定义处理请求的控制器方法，并配置映射路径（(@RequestMapping)与返回json数据(@ResponseBody)

---

- AbstractDispatcherServletInitializer类是SpringMVC提供的快速初始化Web3.O容器的抽象类
- AbstractDispatcherServletInitializer提供三个接口方法供用户实现

1. createServletApplicationContext()方法 创建Servlet容器时，加载SpringMVC对应的bean并放入WebApplicationContext对象范围中 而WebApplicationContext的作用范围为ServletContext范围 即整个web容器范围

```java
protected WebApplicationContext createServletApplicationContext(){
	AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
	ctx.register(SpringMvcConfig.class);
	return ctx;
}
```

2. getServletMappings()方法，设定SpringMVC对应的请求映射路径，设置为/表示拦截所有请求，任意请求都将转入到SpringMVC进行处理
```java
protected String[]getServletMappings(){
	return new String[]{"/"};
}
```

3. createRootApplicationContext()方法，如果创建Servlet容器时需要加载非SpringMVC对应的bean,使用当前方法进行，
使用方式同createServletApplicationContext()
```java
protected WebApplicationContext createRootApplicationContext(){
	return null;
}
```

## Process

入门案例工作流程分析

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-spring-MVC-initialization.png)


启动服务器初始化过程
1. 服务器启动，执行ServletContainersInitConfig类，初始化web容器
2. 执行createServletApplicationContext方法，创建了WebApplicationContext对象
3. 加载SpringMvcConfig
4. 执行@ComponentScan加载对应的bean
5. 加载UserController,每个@RequestMapping的名称对应一个具体的方法
6. 执行getServletMappings方法，定义所有的请求都通过SpringMVC 


单次请求过程

1. 发送请求localhost/save
2. web容器发现所有请求都经过SpringMVC,将请求交给SpringMVC处理
3. 解析请求路径/save
4. 由/save匹配执行对应的方法save()
5. 执行save()
6. 检测到有@ResponseBody直接将save()方法的返回值作为响应体返回给请求方


## Load Control of Bean

Controller加载控制与业务bean加载控制
- SpringMVC相关bean(表现层bean)
- Spring控制的bean
	- 业务bean(Service)
	- 功能bean(DataSource等)

==因为功能不同，所以要避免Spring错误的加载SpringMVC管理的bean==

SpringMVC相关bean加载控制
- springMVC加载的bean对应的包均在com.test.controller包内

Spring相关bean加载控制
1. Spring加载的bean设定扫描范围为com.test,排除掉controller包内的bean
2. Spring加载的bean设定扫描范围为精准范围，例如service包、dao包等
3. 不区分Spring与SpringMVC的环境，加载到同一个环境中

---

名称：@ComponentScan
类型：类注解
范例：
```java
@Configuration
@ComponentScan(value "com.test",
	excludeFilters @ComponentScan.Filter(
		type FilterType.ANNOTATION,
		classes Controller.class
	)
)
public class SpringConfig{
}
```
属性:
- excludeFilters:排除扫描路径中加载的bean,需要指定类别(type)与具体项(classes)
- includeFilters:加载指定的bean,需要指定类别(type)与具体项(classes)


**bean容器的加载格式**

```java
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer{
	protected WebApplicationcontext createServletApplicationContext(){
		AnnotationConfigWebApplicationContext ctx = new AnnotationConfigwebApplicationContext();
		ctx.register(SpringMvcConfig.class);
		return ctx;
	}
	protected WebApplicationContext createRootApplicationcontext(){
		AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
		ctx.register(SpringConfig.class);
		return ctx;
	}
	protected string[] getServletMappings(){
		return new string[]{"/"};
	}
}
```
---

- 简化开发

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
	protected Class<?>[] getServletConfigclasses(){
		return new Class[]{SpringMvcConfig.class};
	}
	protected String[] getservletMappings(){
		return new String[]{"/"};
	}
	protected class<?>[] getRootConfigclasses(){
		return new Class[]{SpringConfig.class};
	}
}
```

# Request & Response

## RequestMapping

名称：@RequestMapping
类型：==方法注解== ==类注解==
位置：SpringMVC控制器方法定义上方
作用：设置当前控制器方法请求访问路径，如果设置在类上统一设置当前控制器方法请求访问路径前缀
范例：
```java
@Controller
@RequestMapping("/user")
public class UserController{
	@RequestMapping("/save")
	@ResponseBody
	public String save(){
		System.out.println("user save ...")
		return "{'module':'user save'}";
	}
}
```
属性
- value(默认)：请求访问路径，或访问路径前缀


## Get & Post



Get请求传参

- 普通参数：url地址传参，地址参数名与形参变量名相同，定义形参即可接收参数


Post请求参数

- 普通参数：form表单post请求传参，表单参数名与形参变量名相同，定义形参即可接收参数

**Post请求中文乱码处理**

- 为web容器添加过滤器并指定字符集，Spring-web包中提供了专用的字符过滤器
```java
public class ServletContainersInitConfig extends AbstractAnnot ationConfigDispatcherServletInitializer{
  //配字符编码过滤器
  @Override
	protected Filter[] getServletFilters(){
		CharacterEncodingFilter filter = new CharacterEncodingFilter();
		filter.setEncoding("utf-8");
		return new Filter[]{filter};
	}
}
```

### Parameters

- 普通参数：请求参数名与形参变量名不同，使用@RequestParam绑定参数关系

---
名称：@RequestParam
类型：形参注解
位置：SpringMVC控制器方法形参定义前面
作用：绑定请求参数与处理器方法形参间的关系
范例：
```java
@RequestMapping("/commonParamDifferentName")
@ResponseBody
public String commonParamDifferentName(@RequestParam("name")String userName int age){
	System.out.println("普通参数传递userName=>"+userName);
	System.out.print1ln("普通参数传递age=>"+age);
	return "{'module':common param different name'}";
}
```
- 参数：
	- required:是否为必传参数
	- defaultValue:参数默认值

---
- POJO参数：请求参数名与形参对象属性名相同，定义POJO类型形参即可接收参数

- 嵌套POJO参数：请求参数名与形参对象属性名相同，按照对象层次结构关系即可接收嵌套POJO属性参数

- 数组参数：请求参数名与形参对象属性名相同且请求参数为多个，定义数组类型形参即可接收参数 

- 集合保存普通参数：请求参数名与形参集合对象名相同且请求参数为多个，@RequestParam绑定参数关系
```java
@RequestMapping("/listParam")
@ResponseBody
public String listParam(@RequestParam List<String>likes){
	System..out.print1n("集合参数传递likes=>"+likes);
	return "{'module':'list param'}";
}
```

---

请求参数（传递json数据）
- json数组
- json对象(POJO)
- json数组(POJO)

1. 添加json数据转换相关坐标
```xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.9.0</version>
</dependency>
```
2. 设置发送json数据 (请求body中添加json数据)

3. 开启自动转换json数据的支持
```java
@Configuration
@ComponentScan("com.test.controller")
@EnableWebMvc
public class SpringMvcConfig{
}
```

注意事项
- @EnablewebMvc注解功能强大，该注解整合了多个功能，此处仅使用其中一部分功能，即json数据进行自动类型转换

4. 设置接收json数据
```java
@RequestMapping("/listParamForJson")
@ResponseBody
public String listParamForJson(@RequestBody List<String>likes){
	System.out.println("1ist common(json)参数传递1ist=>"+likes);
	return "{'module':'list common for json param'}";
}
``` 

---

**名称：@EnableWebMvc**
类型：配置类注解
位置：SpringMVC配置类定义上方
作用：开启SpringMVC多项辅助功能
范例：
```java
@Configuration
@ComponentScan("com.test.controller")
@EnableWebMvc
public class SpringMvcConfig{
}
```
---
**名称：@RequestBody**
类型：形参注解
位置：SpringMVC控制器方法形参定义前面
作用：将请求中请求体所包含的数据传递给请求参数，==此注解一个处理器方法只能使用一次==
范例：
```java
@RequestMapping("/listParamForJson")
@ResponseBody
public String listParamForJson(@RequestBody List<String>likes){
	System.out.printIn("1 ist common(json)参数传递list=>"+likes);
	return "'module':'list common for json param'}";
}
```
---

**@RequestBody与@RequestParam区别**

- 区别
	- @RequestParam用于接收url地址传参，表单传参【application/x-www-form-urlencoded】
	- @RequestBody用于接收json数据【application/json】
- 应用
	- 后期开发中，发送json格式数据为主，@RequestBody,应用较广
	- 如果发送非json格式数据，选用@RequestParam接收请求参数

---

**日期类型参数传递**

- 日期类型数据基于系统不同格式也不尽相同
	- 2088-08-18
	- 2088/08/18
	- 08/18/2088

- 接收形参时，根据不同的日期格式设置不同的接收方式
```java
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Datedate,
												@DateTimeFormat(pattern "yyyy-MM-dd")Date date1,
												@DateTimeFormat(pattern "yyyy/MM/dd HH:mm:ss")Date date2){
	System.out.println("参数传递date=>"+date);
	System.out.println("参数传递date(yyyy-MM-dd)=>"+date1);
	System..out.println("参数传递date(yyyy/MM/ddHH:mm:ss)=>"+date2);
	return "{'module':'data param'}";
}
```
```http
http://1oca1host/dataParam?date=2088/08/08&date1=2888-08-18&date2=2888/08/288:08:08
```

---
日期类型参数传递

名称：**@DateTimeFormat**
类型：形参注解
位置：SpringMVC:控制器方法形参前面
作用：设定日期时间型数据格式
范例：
```java
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date){
System.out.println("参数传递date==>"+date);
return "{'module':'data param'}";
```
-  属性：pattern:日期时间格式字符串

---

类型转换器
- Converter接口
	- 请求参数年龄数据(String→Integer)
	- 日期格式转换(String→Date)

```java
public interface Converter<S,T>{
	@Nullable
	T convert(S var1);
}
```
- @EnableWebMvc功能之一：根据类型匹配对应的类型转换器

## Response

响应

**响应页面**
```java
@RequestMapping("/toPage")
public String toPage(){
	return "page.jsp";
}
```
 
**响应数据**
- 文本数据
```java
@RequestMapping("/toText")
@ResponseBody
public String toText(){
	return "response text";
} 
```
- json数据

对象转json
```java
@RequestMapping("/toJsonPOJo")
@ResponseBody
public User toJsonPOJO(){
	Useruser new User();
	user.setName("赵云");
	user.setAge(41);
	return user;
}
```

对象集合转json数组
```java
@RequestMapping("/toJsonList")
@ResponseBody
public List<User>toJsonList(){
	User user1 new User();
	user1.setName("赵云");
	user1.setAge(41);
	User user2 new User();
	user2.setName("master赵云");
	user2.setAge(40);
	List<User>userList new ArrayList<User>();
	userList.add(user1);
	userList.add(user2);
	return userList;
}
```

---
名称：**@ResponseBody**
类型：方法注解
位置：SpringMVC控制器方法定义上方
作用：==设置当前控制器方法响应内容为当前返回值，无需解析==
范例：
```java
@RequestMapping("/save")
@ResponseBody
public String save(){
	System.out.printin("save...");
	return "{'info':'springmvc'}";
}
```
---
- HttpMessageConverter接口
```java
public interface HttpMessageConverter<T>{
	boolean canRead(Class<?>clazz,@Nullable MediaType mediaType);
	boolean canWrite(Class<?>clazz,@Nullable MediaType mediaType);
	List<MediaType>getSupportedMediaTypes();
	T read(Class<?extends T>clazz,HttpInputMessage inputMessage)
				throws IOException,HttpMessageNotReadableException;
	void write(Tt,@Nullable MediaType contentType,HttpOutputMessage outputMessage)
				throws IOException,HttpMessageNotWritableException;
}
```

# REST

## Introduction

REST简介

REST(Representational State Transfer),表现形式状态转换

传统风格资源描述形式
```http
http://localhost/user/getById?id=1
http://localhost/user/saveUser
```

REST风格描述形式
```html
http://localhost/user/1
http://localhost/user
```
优点：
- 隐藏资源的访问行为，无法通过地址得知对资源是何种操作
- 书写简化

---

- 按照REST风格访问资源时使用==行为动作==区分对资源进行了何种操作
	- http://localhost/users 查询全部用户信息  GET （查询）
	- http://localhost/users/1 查询指定用户信息 GET （查询）
	- http://localhost/users 添加用户信息 POST （新增/保存）
	- http://localhost/users 修改用户信息 PUT （修改/更新）
	- http://localhost/users/1 删除用户信息 DELETE （删除）

- 根据REST风格对资源进行访问称为RESTfu1

---

**注意事项**

上述行为是约定方式，约定不是规范，可以打破，所以称REST风格，而不是REST规范

==描述模块的名称通常使用复数==，也就是加s的格式描述，表示此类资源，而非单个资源，例如：users、books、accounts.…

## Get Started

1. 设定http请求动作（动词）
```java
@RequestMapping(value "/users",method = RequestMethod.POST)
@ResponseBody
public String save(@RequestBody User user){
	System.out.println("user save..."+user);
	return "{'module':'user save'}";
}

@RequestMapping(value "/users",method = RequestMethod.PUT)
@ResponseBody
public String update(@RequestBody User user){
	System.out.println("user update..."+user);
	return "{'module':'user update'}";
}
```

2. 设定请求参数（路径变量）
```java
@RequestMapping(value "/users/{id}",method RequestMethod.DELETE)
@ResponseBody
public String delete(@PathVariable Integer id){
	System.out.println("user delete..."+id);
	return "{'module':'user delete'}";
}
```

---
名称：**@RequestMapping**
类型：方法注解
位置：SpringMVC控制器方法定义上方
作用：设置当前控制器方法请求访问路径
**属性**
- value(默认)：请求访问路径
- method:http请求动作，标准动作(GET/POST/PUT/DELETE)

---

名称：**@PathVariable**

类型：形参注解
位置：SpringMVC控制器方法形参定义前面
作用：绑定路径参数与处理器方法形参间的关系，要求路径参数名与形参名一一对应

---

==三种接收参数方式总结==

@RequestBody @RequestParam @PathVariable

区别：

- @RequestParam用于接收url地址传参或表单传参
- @RequestBody用于接收json数据
- @PathVariable用于接收路径参数，使用{参数名称]描述路径参数

应用
- 后期开发中，发送请求参数超过1个时，以json格式为主 @RequestBody应用较广
- 如果发送非json格式数据，选用@RequestParam接收请求参数
- 采用RESTful进行开发，当参数数量较少时，例如1个，可以采用@PathVariable接收请求路径变量，通常用于传递id值

---

## Rapid Development

RESTfu1快速开发

---
名称：**@RestController**
类型：类注解
位置：基于SpringMVCl的RESTful开发控制器类定义上方
作用：设置当前控制器类为RESTful风格，等同于@Controller与@ResponseBody两个注解组合功能
范例：
```java
@RestController
public class BookController{
}
```
---
名称：**@GetMapping@PostMapping@PutMapping@DeleteMapping**
类型：==方法注解==
位置：基于SpringMVC的RESTful开发控制器方法定义上方
作用：设置当前控制器方法请求访问路径与请求动作，每种对应下个请求动作，例如@GetMapping对应GET请求
范例：
```java
@GetMapping("/{id}")
public String getById(@PathVariable Integer id){
	System.out.println("book getById..."+id);
	return "{'module':'book getById'}";
}
```
属性:
- value(默认)：请求访问路径

## Case

案例：基于RESTful页面数据交互

1. 制作SpringMVC控制器，并通过PostMan测试接口功能
```java
@RestController
@RequestMapping("/books")
public class BookController
	@PostMapping
	public String save(@RequestBody Book book){
		System.out.println("book save ==>"book);
	return "{'module':'book save success'}";
	}
	@GetMapping
	public List<Book>getAll(){
		System.out.printin("book getAll is running ...")
		List<Book>bookList new ArrayList<Book>();
		Book book1 new Book();
		book1.setType("计算机")；
		book1.setName("SpringMVC入门教程")；
		book1.setDescription("小试牛刀")；
		bookList.add(book1);
		//模拟数据 ...
		return bookList;
	}
}
```

2. 设置对静态资源的访问放行
```java
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport
@Override
	protected void addResourceHandlers(ResourceHandlerRegistry registry){
		//当访问/pages/??时候，走/pages目录下的内容
		registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");	
		registry.addResourceHandler("/js/**").addResourceLocations("/js/");
		registry.addResourceHandler("/css/**").addResourceLocations("/css/");
		registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
	}
}
```
3. 前端页面通过异步提交访问后台控制器
```java
//添加
saveBook () {
	axios.post("/books",this.formData).then((res)=>{
	});
},

//主页列表查询
getAll(){
	axios.get("/books").then((res)=>{
	this.dataList = res.data;
	});
},
``` 
# Integration

## SSM

### Process

SSM整合流程
1. 创建工程
2. SSM整合
- Spring
	- SpringConfig
- MyBatis
	- MybatisConfig
	- JdbcConfig
	- jdbc.properties
- SpringMVC
	- ServletConfig
	- SpringMvcConfig
3. 功能模块
- 表与实体类
- dao(接口+自动代理)
- service(接口+实现类)
	- 业务层接口测试（整合JUnit)
- controller
	- 表现层接口测试(PostMan)

---

### MyBatis

1. 配置

**SpringConfig**

```java
@Configuration
@ComponentScan("com.test")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
public class SpringConfig
}
```

**JDBCConfig, jdbc.properties**

```java
//JDBCConfig
public class JdbcConfig{
	@Value("$jdbc.driver}")
	private String driver;
	@Value("${jdbc.ur1}")
	private String url;
	@Value("${jdbc.username}")
	private String userName;
	@Value("${jdbc.password}")
	private String password;
	@Bean
	public DataSource dataSource(){
		DruidDataSource ds = new DruidDataSource();
		ds.setDriverClassName(driver);
		ds.seturl(url);
		ds.setUsername(userName);
		ds.setPassword(password);
		return ds;
	}
}
```
```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm db?useSSL=false
jdbc.username=root
jdbc.password=root
```

**MyBatisConfig**
 
```java
public class MybatisConfig {
	@Bean
	public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
		SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
		ssfb.setTypeAliasesPackage("com.test.domain");
		ssfb.setDatSource(dataSource);
		return ssfb;
	}
@Bean
	public MapperScannerConfigurer mapperScannerConfigurer(){
		MapperScannerConfigurer msc new MapperScannerConfigurer();
		msc.setBasePackage("com.test.dao");
		return msc;
	}
}
```
2. 模型

**Book**

```java
public class Book{
	private Integer id;
	private String type;
	private string name;
	private String description;
}
```

3. 数据层标准开发

**BookDao**

```java
public interface BookDao{
	@Insert("insert into tbl_book(type,name,description)values(#{type},#{name),#{description))")
	void save(Book book);
	@Delete("delete from tbl_book where id #id}"
	void delete(Integer id);
	@Update("update tbl_book set type-#type},name-#(name},description-#description}where id-#id)")
	void update(Bookbook);
	@Select("select from tbl book")
	List<Book>getAll();
	@Select("select from tbl_book where id #fid}"
	Book getById(Integer id);
}
```

4. 业务层标准开发

**BookService**

```java
public interface BookService {
	void save(Book book);
	void delete(Integer id);
	void update(Bookbook);
	List<Book>getAll();
	Book getById(Integer id);
}
```

**BookServiceImpl**
```java
@Service
public class BookServiceImpl implements BookService{
	@Autowired
	private BookDao bookDao;
	public void save(Bookbook){
		bookDao.save(book);
	}
	public void update(Book book){
		bookDao.update(book);
	}
	public void delete(Integer id){
		bookDao.delete(id);
	}
	public Book getById(Integer id){
		return bookDao.getById(id);
	}
	public List<Book>getAll(){
		return bookDao.getAl1();
	}
}
```

5. 测试接口

**BookServiceTest**
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest {
	@Autowired
	private BookService bookService;
	@Test
	public void testGetdById(){
		System.out.println(bookService.getById(1));
	}
	@Test
	public void testGetAll(){
		System.out.println(bookService.getAll());
	}
}
```

6. 事务处理

```java
public class JdbcConfig {
	@Bean
	public PlatformTransactionManager transactionManager(DataSource dataSource){
		DataSourceTransactionManager transactionManager = new DataSourceTransactionManager(dataSource);
		return transactionManager;
	}
}
```
```java
@Configuration
@ComponentScan("com.test")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
@EnableTransactionManagement
public class SpringConfig{
}
```
```java
@Transactional
public interface BookService {
}
```

### SpringMVC

**web配置类**

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer{
	protected Class<?>[] getRootConfigclasses(){
	return new class[]{SpringConfig.class};
	}
	protected Class<?>[] getServletConfigclasses(){
		return new class[]{SpringMvcConfig.class};
	}
	protected String[] getservletMappings(){
		return new String[]{"/"};
	}
	//乱码处理
	@Override
	protected Filter[]getServletFilters(){
		CharacterEncodingFilter filter new CharacterEncodingFilter();
		filter.setEncoding("UTF-8");
		return new Filter[]{filter};
	}
}
```

**SpringMVC配置类**

```java
@Configuration
@ComponentScan({"com.test.controller"})
@EnableWebMvc
public class SpringMvcConfig {
)
```

**基于Restful的Controller开发**

```java
@RestController
@RequestMapping("/books")
public class BookController{
	@Autowired
	private BookService bookService;
	@PostMapping
	public void save(@RequestBody Book book){
		bookService.save(book);
	}
	@PutMapping
	public void update(@RequestBody Book book){
		bookService.update(book);
	}
	@DeleteMapping("/{id}")
	public void delete(@PathVariable Integer id){
		bookService.delete(id);
	} 
	@GetMapping("/{id}")
	public Book getById(@PathVariable Integer id){
		return bookService.getById(id);
	}
	@GetMapping
	public List<Book>getAll(){
		return bookService.getAll();
	}
}
```

## Process Controller Data 

**设置统一数据返回结果类**

```java
public class Result{
	private object data;
	private Integer code;
	private string  msg;
}
```

==注意事项==

Result类中的字段并不是固定的，可以根据需要自行增减
提供若干个构造方法，方便操作

---

**设置统一数据返回结果编码**
```java
public class Code {
public static final Integer SAVE_OK = 20011;
public static final Integer DELETE_OK = 20021;
public static final Integer UPDATE_OK = 20031;
public static final Integer GET_OK = 20041;
public static final Integer SAVE_ERR = 20010;
public static final Integer DELETE_ERR = 20020;
public static final Integer UPDATE_ERR = 20030;
public static final Integer GET_ERR = 20040;
}
```
==注意事项==

Code类的常量设计也不是固定的，可以根据需要自行增减，例如将查询再进行细分为GET_OK,GET_ALL_OK,GET_PAGE_OK

---

**根据情况设定合理的Result**
```java
@RequestMapping("/books")
public class BookController {
	@Autowired
	private BookService bookService;
	@GetMapping("/id}")
	public Result getById(@PathVariable Integer id){
		Book book bookService.getById(id);
		Integer code= book != null Code.GET OK Code.GET ERR;
		String msg = book != null? " "" : "数据查询失败,请重试";
		return new Result(code,book,msg);
	}
}
```