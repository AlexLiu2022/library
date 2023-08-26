---
{"dg-publish":true,"permalink":"/tree/mark-down/","tags":["CS/mark-up-languages"],"created":"2022-08-15T01:17:14.164+08:00","updated":"2023-08-27T04:54:41.810+08:00"}
---


# Introduction

Markdown is a lightweight markup language for creating formatted text using a plain-text editor. John Gruber and Aaron Swartz created Markdown in 2004 as a markup language that is appealing to human readers in its source code form. Markdown is widely used in blogging, instant messaging, online forums, collaborative software, documentation pages, and readme files.

---

Sntax are within the note of a very popular markdown editor

 Mark-down Sntax :


### 标题

1～6个\#表示标题级别1～6



### 加粗

代码为\**text**  

### 倾斜

代码为 \*text* 

### 下划线

代码为 \<u>text\<u/> 

(这不是markdown语法 而是HTML)

### 组合使用

eg.**<u>*我是具有下划线且被加粗了的斜体字*</u>**

### ==高亮==

代码为\==text\== 

### 上下标

#### 上标

代码为 \^text\^  效果为 m^3^

#### 下标

代码为\~text\~ 效果为 H~2~O

### 列表

#### 无序列表

- 代码为  +/- /*   +   空格  
	

#### 有序列表

1. 代码为 1（数字）\. + 空格 + text  


### 任务列表

#### 未完成状态 

源代码为 -空格[空格]空格text 

- [ ] 未完成状态任务列表效果

#### 完成状态

源代码为 -空格[x]空格text  

- [x] 完成状态任务列表效果

### 代码

#### 行内代码

源代码为 \`code\`  

行内代码的效果 `int`

#### 代码块

源代码为

 \```language            

Code

\```

代码块的效果如下

```go
//声明源文件属于的package
package main
//引入标准库"fmt"
import"fmt"
```

### 公式

#### 行内公式

源代码为 \$LaTex$  

效果为 $sinx$ 

#### 公式块

源代码为

\$$

LaTex       

\$$


效果如下
$$
E_{\rm k} = \frac 1 2 m v^2
\tag{1.1}
$$

用行内公式引用公式 $(1.1)$

### 表格

源代码 如下

\|    a  |  b    |    c  |
\| ---- | ---- | ---- |
\|   1  |    2 |      3|
\|   1  |    2 |      3|
\|   1  |    2 |      3|


效果如下

| a   | b   | c   |
| --- | --- | --- |
| 1   | 2   | 3   |
| 1   | 2   | 3   |
| 1   | 2   | 3   |



### 引用

源代码是 \>空格text 

效果如下

> 隐约雷鸣 忽震迟滞心绪 
>
> 神宫何在 苦寄下士逆旅
>
> 若见天光 莫辞独步孤路
>
> 知止应是 笑定明日乾坤               ——Re[^1]

### 超链接

#### 网页

源代码是 \[text](URL). 


效果为 [多多的博客](https://freezing.cool) 

####  本文

源代码是 \[text](#name) 


效果为 [回到开头](#基本操作)

 ### 插入图片

picgo/ipic

### 脚注

源代码为

text \[^tag] 

……                



\[^tag]:text

效果见[这里的作者](#引用)



### 分割线

源代码为 三个或更多的 \--- 


效果如下

---

### 显示语法符号自身

在符号前+反斜线 \ 效果如上所有源代码处

[^1]:I myself

