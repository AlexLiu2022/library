---
{"dg-publish":true,"dg-path":"  maven.md","permalink":"/maven/","tags":["CS/programming-languages/java","tools/project-management"],"created":"2022-08-07T04:01:46.236+08:00","updated":"2023-09-14T17:17:32.481+08:00"}
---


# Introduction 

Maven是专门用于管理和构建java项目的工具，它的主要功能有：

- 提供了一套标准化的项目结构
	- 所有IDE使用Maven构建的项目结构完全一样，所有IDE创建的Maven项目可以通用
- 提供了一套标准化的构建流程(编译，测试，打包，发布…)
- 提供了一套依赖管理机制
	- 依赖管理就是管理项目所依赖的第三方资源(Jar包、插件.…)
	1. Maven使用标准的坐标配置来管理各种依赖
	2. 只需要简单的配置就可以完成依赖管理

---

下图是Maven项目结构示例：

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/maven-project-structure.png)

---

# Basic

---

 ## Maven模型
- 项目对象模型(Project Object Model)

- 依赖管理模型(Dependency)
 
- 插件(Plugin)

## Maven仓库

仓库分类：

- 本地仓库：自己计算机上的一个目录

- 中央仓库：由Maven团队维护的全球唯一的仓库 
	- 地址：https://repo1.maven.org/maven2/

- 远程仓库（私服）：一般由公司团队搭建的私有仓库

项目中使用坐标引入对应依赖后，首先会查找本地仓库中是否有对应的jar包：

- 如果有，则在项目直接引用：

- 如果没有，则去中央仓库中下载对应的jar包到本地仓库。

还可以搭建远程仓库，将来jar包的查找顺序则变为：

本地仓库→远程仓库→中央仓库


## Maven安装配置


1. 解压apache-maven-3.6.1.rar既安装完成
2. 配置环境变量MAVEN HOME为安装路径的bin目录
3. 配置本地仓库：修改conf/settings.xml中的\<localRepository>为一个指定目录
4. 配置阿里云私服：修改conf/settings.xml中的\<mirrors>标签，为其添加如下子标签：

```xml
<mirror>
<id>alimaven</id>
<name>aliyun maven</name>
<url>https://maven.aliyun.com/repository/public/</url>
<mirrorOf>central</mirrorOf>
</mirror>
```

## Maven常用命令

- complie
- clean
- test
- package
- install 

## Maven生命周期

Maven构建项目生命周期描述的是一次构建过程经历经历了多少个事件

Maven对项目构建的生命周期划分为3套
- clean:清理工作
- default:核心工作，例如编译，测试，打包，安装等
- site:产生报告，发布站点等
 
==同一生命周期内，执行后边的命令，前边的所有命令会自动执行==

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/maven-life-circle.png)

---

## IDEA配置Maven

---

### IDEA配置Maven环境

1. 选择IDEA中File->Settings
2. 搜索maven
3.  设置IDEA使用本地安装的Maven,并修改配置文件路径

### Maven坐标详解
- 坐标：
	- Maven中的坐标是资源的唯一标识

	- 使用坐标来定义项目或引入项目中需要的依赖
- Maven坐标主要组成：
	- groupld:
		- 定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.AlexLiu)
	- artifactld:
		- 定义当前Maven项目名称（通常是模块名称，例如order-service、goods-service)
	- version:
		- 定义当前项目版本号

### IDEA创建Maven项目

1. 创建模块，选择Maven,点击Next
2. 填写模块名称，坐标信息，点击finish,创建完成
3. 编写HelloWorld,并运行

#### Maven Web项目

Web项目结构：

- Maven Web 项目结构：开发中的项目

---

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/developing-maven-web-project.png)

---
- 部署的JavaWeb项目结构：开发完成，可以部署的项目

---

![](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/finished-maven-web-project.png)

---
- 编译后的Java字节码文件和resources的资源文件，放到WEB-lNF下的classes目录下
- pom.xml中依赖坐标对应的jar包，放入WEB-lNF下的Iib目录下

---

创建项目

- 使用archetype

1. 选择web archetype，创建项目
2. 删除pom.xml中多余的坐标
3. 补齐缺失的目录结构

- 不使用archetype

1.  创建项目
2. pom.xml中添加打包方式为war
3. 补齐缺失的目录结构：webapp

### IDEA导入Maven项目

1. 选择右侧Maven面板，点击+号
2. 选中对应项目的pom.xml文件，双击即可
3. 如果没有Maven面板，选择  View→Appearance→Tool Window Bars


## 依赖管理

使用坐标导入jar包

1. 在pom.xml中编写\<dependencies>标签
2. 在\<dependencies>标签中使用\<dependency>引入坐标
3. 定义坐标的`groupld`,`artifactld`,`version`
4. 点击刷新按钮，使坐标生效

依赖范围

通过设置坐标的依赖范围(scope),可以设置对应jar包的作用范围：编译环境、测试环境、运行环境
```xml
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.13</version>
<scope>test</scope>
</dependency>
```
| 依赖范围 | 编译classpath            | 测试classpath | 运行classpath | 例子              |     |
| -------- | ------------------------ | ------------- | ------------- | ----------------- | --- |
| compile  | Y                        | Y             | Y             | logback           |     |
| test     | -                        | Y             | -             | Junit             |     |
| provided | Y                        | Y             | -             | servlet-api       |     |
| runtime  | Y                        | Y             | Y             | jdbc驱动          |     |
| system   | Y                        | Y             | -             | 存储在本地的jar包 |     |
| import   | 引入DependencyManagement |               |               |                   |     |

---

**\<scope>:默认值：compile**

# Advanced 