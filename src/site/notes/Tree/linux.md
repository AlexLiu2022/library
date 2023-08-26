---
{"dg-publish":true,"permalink":"/tree/linux/","created":"2022-07-31T06:48:34.422+08:00","updated":"2023-08-26T19:56:56.885+08:00"}
---


- Use [[Tree/shell\|shell]] script  to execute commands in terminal.

- The native editor of Linux is [[Tree/vim\|vim]].

---

# Linux

## Linux目录结构

### 根目录 /

- /bin(binary)（usr/bin、/usr/local/bin）存放着最常使用的命令
- /sbin(usr/sbin、usr/local/sbin)存放系统管理员使用的系统管理程序
- /home 存放普通用户的主目录 Linux中每个用户拥有一个以自己用户名为名的文件夹
- /root 系统管理员/超级权限者的用户主目录
- /lib 系统开机所需的最基本的动态连接共享库
- /lost+found 通常为空 系统非法关机后存放文件 
- /etc 所有系统管理所需要的配置文件和子目录
- /usr 存放用户的很多应用程序和文件
- /boot 存放启动Linux的核心文件 包括连接文件及镜像文件
- /proc 虚拟目录 系统内存的映射 访问此目录获取系统信息
- /srv（service)存放一些服务启动后需要提取的数据 
- /sys 该目录下安装了文件系统sysfs
- /tmp 存放临时文件
- /dev 把所有的硬件以文件的形式存储
- /media Linux将自动识别的设备挂载到此目录下
- /mnt 挂载别的文件系统，可将外部存储挂载在该目录中
- /opt 存 主机额外安装软件的目录 
- /usr/local 另一个给主机额外安装软件安装的目录，一般是通过编译源码方式安装的程序
- /var 存放不断扩充的东西，习惯将经常被修改的目录放在此目录下，包括各种日志文件

## 远程使用Linux

### Linux远程登陆（ssh)

### Linux远程文件传输(ftp)

## Linux实际操作



