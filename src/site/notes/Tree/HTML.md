---
{"dg-publish":true,"permalink":"/tree/html/","tags":["CS/web","CS/mark-up-languages"],"created":"2022-08-15T01:23:31.812+08:00","updated":"2023-08-27T03:00:22.691+08:00"}
---


The HyperText Markup Language or HTML is the standard markup language for documents designed to be displayed in a web browser. It can be assisted by technologies such as Cascading Style Sheets ([[Tree/CSS\|CSS]]) and scripting languages such as [[Tree/javascript\|javascript]].


 Web标准的制定 ——W3C

# HTML的基本语法

## 文档声明与字符编码

## 标签

### 分类

- <常规标记>也叫双标记

<标记></标记>
 <标记 属性=“属性值” 属性=“属性值”></标记>
 
 标记也可叫标签或叫元素
 
 例如：\<head>\</head>

- 空标记也叫单标记 

<标记/> <标记 属性=“属性值” />
 例如：\<br/>

### 常用标签

#### 文本标题(h1-h6)

```html
		<h1>一级标题</h1>
		<h2>二级标题</h2>
	  <h3>三级标题</h3>
		<h4>四级标题</h4>
	  <h5>五级标题</h5>
	  <h6>六级标题</h6>
```

 注：文本标题标签自带加粗，有自己的文本大小，并且独占一行，有默认间距

#### 段落文本(P)
```html
		 <p>段落文体内容<p>
		 标识一个段落（段落与段落之间有段间距）
```
#### 换行(br)

`<br/>`

换行是一个空标记（强制换行）

#### 水平线

 `<hr/>`空标记

#### 加粗

有两个标记（推荐strong)


```html
	 <b>加粗内容</b>                只是显示加粗
	 <strong>强调的内容</strong>     突出的文本
```

 #### 倾斜
 
 有两个标记（推荐em)
 
```html
 <em>强调文本</em>
 <i>倾斜文本<i>
```

#### 删除线

有两个标记（推荐del)
```html
 <s>文本</s>删除线
 <del>文本</del>删除线
```
#### 上下标和下划线

```html
 <u>文本<u>下划线
 <sub></sub>下标
 <sup></sup>上标
```


### div和span标签

- div标签，没有具体含义，用来划分页面的区域，独占一行。
- span标签,没有实际意义，主要应用在对于文本独立修饰的时候，内容有多宽就占用多宽的空间距离。

### 图片标签

属性:
```html
<img src="图片路径" title=“鼠标悬停上去之后的提示信息〞 alt=“图片不显示之后 （加载失败）的提示信息〞 width="200px” height="200px"/>
```

### 超链接标签

 能够实现不同页面的跳转
 ```html
 
 <a href=“路径” title=“鼠标悬停上去之后的提示信息” target=“规定在何处打开文档"> 内容 </a>
 ```
 Target属性：规定在何处打开文档
 - target=“\_self"  默认值
- target=“\_blank" 新窗口打开

### 列表

 #### 有序列表
```html
	 <ol type="A" start="4">
	 <li>有序列表<li>
	 <li>有序列表<li>
	 </ol>
```
 type类型 start开始(取值只是是数字)

 #### 无序列表
 
```html
	 <ul>
	 type = disc circle square
none
	 <li>无序列表<li>
	 <li>无序列表<Ii>
	 <ul>
```

 #### 自定义列表
```html
	 <dl>
	 <dt>可以是文字也可以图</dt>
	 <dd>相关文字</dd>
	</dl>
```

### table表格

#### 基本结构

**tr表示行**

tr属性

 - 高度height
 - 背景颜色bgcolor
 - 文字水平对齐 align=“left或right或center'”
 - 文字垂直对齐 valign=“top或middle或bottom"

**td表示列**

table属性

- 宽度width
- 高度height
- 边框border
- 边框颜色bordercolor
- 背景颜色bgcolor
- 水平对齐align=“left或right或center"
- cellspacing="单元格与单元格之间的间距
- cellpadding="单元格与内容之间的空隙"

#### 表格合并

- Colspan=“所要合并的单元格的列数”必须给td
- Rowspan=“所要合并的单元格的行数”必须给td

### 表单

- 作用 ：收集用户信息

- 结构:

```html
	 <form method="get或者post" action="向何处发送表单数据">


Form当中method的post和get的区别:

1. get是从服务器上获取数据，post是向服务器传送数据。
2. get是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。
3. 对于get方式，服务器端用Request.QueryString?获取变量的值，对于post方式，服务器端用Request.Form获取提交的数据。
4. get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，IS4(Internet
Information Service互联网信息服务)中最大量为80KB,IS5中为100KB
5. get安全性非常低，post安全性较高。但是执行效率却比Post方法好。

   <input/>

 A.属性type定义输入框的类型

	 a)文本框type="text"  密码框type=“password" 
	 b)提交框type="submit"和                      
	<button>提交按钮<button>一样  提交信息到action指定到地址
	 c)按钮框type="button" 单纯的按钮
	 d)重置框type="reset" 清空的效果

 B.属性placeholder描述输入字段预期值的简短的提示信息。兼容到IE8以上
 C.属性name必须设置，否则在提交表单时，用户在其中输入的数据不会被发送给服务器
 D.属性value

</form>
```

## 特殊符号

| 特殊符号 | 解释                                                  | 
| -------- | ----------------------------------------------------- |
| 尖角号   | \&lt;左尖角号                                           |
|          |\&gt;右尖角号                                          |
| 空格     | \&nbsp;该空格占据宽度受【字体】影响明显而强烈          |
|          | \&emsp;占据的宽度正好是1个中文宽度且基本上不受字体影响 |
| 版权     | \&copy;©                                               |
| 商标     | \&trade;TM \&reg;®                                      |

