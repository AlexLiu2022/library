---
{"dg-publish":true,"permalink":"/tree/css/","tags":["CS/web"],"created":"2022-08-15T02:34:30.097+08:00","updated":"2023-08-27T03:01:45.443+08:00"}
---

More detalis see [here](https://developer.mozilla.org/en-US/docs/Web/CSS)   (MDN web Docs on CSS)

# Introduction

英文全名：cascading style sheets = cascading style sheet 层叠样式表

WEB标准中的表现标准语言 

# Use CSS

- 引用方法

	- 内部样式表

		- 1)每个CSS样式由两部分组成，即选择符和声明，声明又分为属性和属性值
		- 2)属性必须放在花括号中，属性与属性值用冒号连接。
		- 3)每条声明用分号结束。
		- 4)当一个属性有多个属性值的时候，属性值与属性值不分先后顺序，用空格隔开。
		- 5)在书写样式过程中，空格、换行等操作不影响属性显示

	- 外部样式表
	- 行内样式表

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
	<meta charset="UTF-8">
	<title>css三种引入方式</title>
	<!-- 三种 内部/外部/行内-->
	<link rel="stylesheet" type = "text/css"href="01.css">
</head>
<body>
	<!-- 行内式： -->
	<!-- 1. 在标签头部的style属性内 -->
	<!-- 2. 属性值满足的是 css语法 -->
	<!-- 3. 属性值用key:value 形式赋值，value 具有单位 -->
	<!-- 4. 属性值之间用；隔开 -->
	<div style="width:100px;height:100px;background-color: red">test</div>
	<!-- 内部式 -->
	<!-- 1.在style标签内（style标签一般为head的子标签 -->
	<!-- 2.属性值满足的是 css语法 -->
	<!-- 3. 属性值用key:value 形式赋值，value 具有单位 -->
	<!-- 4. 属性值之间用；隔开 -->
	<style type="text/css">
		div{
			width: 200px;
			height:200PX;
			background-color: brown;
		}
	</style>

	<!-- 外部式 -->
	<!-- 1.在外部css文件中 -->
	<!-- 2.属性值满足的是css语法 -->
	<!-- 3.属性值用key：value 形式赋值，value具有单位 -->
	<!-- 4.属性值之间用；隔开（一般独行分开赋值） -->
	<!-- 5.格式： div{样式块} -->
	<!-- 将html 与 css文件建立联系：html通过link标签链接外部css （一般在head中链接）-->
	<!-- <div></div> -->
	<!-- <link rel="stylesheet" type = "text/css"href="01.css"> -->
</body>
</html>
```

---



