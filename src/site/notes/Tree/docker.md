---
{"dg-publish":true,"permalink":"/tree/docker/","tags":["CS/microservices/middlewares"],"created":"2022-09-10T16:03:55.456+08:00","updated":"2023-08-27T05:13:41.629+08:00"}
---


# Introduction

## 原理

Docke解决大型项目依赖关系复杂，不同组件依赖的兼容性问题方法：

- Docker允许开发中将应用、依赖、函数库、配置一起打包，形成可移- 植镜像
- Docker应用运行在容器中，使用沙箱机制，相互隔离

Docker解决开发、测试、生产环境有差异问题方法：
- Docker镜像中包含完整运行环境，包括系统函数库，仅依赖系统的Linux内核，因此可以在任意Liux操作系统上运行

## docker与虚拟机

虚拟机(virtual machine)是在操作系统中模拟硬件设备，然后运行另一个操作系统，比如在Windows系统里面运行Ubuntu系统，这样可以运行任意的Ubuntu应用

---

| 特性     | Docker   | 虚拟机   |
| -------- | -------- | -------- |
| 性能     | 接近原生 | 性能较差 |
| 硬盘占用 | 一般为MB | 一般为GB |
| 启动     | 秒级     | 分钟级   |

---

Docker和虚拟机的差异：
- docker是一个系统进程；虚拟机是在操作系统中的操作系统
- docker体积小、启动速度快、性能好；虚拟机体积大、启动速度慢、性能一般


## 镜像和容器
- 镜像(Image)：Docker将应用程序及其所需的依赖、函数库、环境、配置等文件打包在一起，称为镜像
- 容器(Container):镜像中的应用程序运行后形成的进程就是容器，只是Docker会给容器做隔离，对外不可见

## Docker & DockerHub

DockerHub:DockerHub是一个Docker镜像的托管平台。这样的平台称为Docker Registry。
国内也有类似于DockerHub的公开服务，比如网易云镜像服务、阿里云镜像库等。

## docker架构
Docker是一个CS架构的程序，由两部分组成：
- 服务端(server):Docker守护进程，负责处理Docker指令，管理镜像、容器等
- 客户端(client):通过命令或RestAPI向Docker服务端发送指令。可以在本地或远程向服务端发送指令