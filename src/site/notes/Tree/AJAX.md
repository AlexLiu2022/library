---
{"dg-publish":true,"permalink":"/tree/ajax/","tags":["CS/web"],"created":"2022-08-14T16:30:47.411+08:00","updated":"2023-08-27T03:03:48.678+08:00"}
---


Easy Way to use AJAX, see [[Tree/axios\|axios]]

---

# Introduction

---
Asynchronous [[Tree/javascript\|javascript]] and [[Tree/XML\|XML]], while ==not a technology in itself==, is a term coined in 2005 by Jesse James Garrett, that describes a "new" approach to using a number of existing technologies together, including HTML or XHTML, CSS, JavaScript, DOM, XML, XSLT, and most importantly the `XMLHttpRequest` object. When these technologies are combined in the Ajax model, web applications are able to make quick, incremental updates to the user interface without reloading the entire browser page. This makes the application faster and more responsive to user actions.

**Although X in Ajax stands for XML, JSON is preferred over XML nowadays** because of its many advantages such as being a part of JavaScript, thus being lighter in size. Both JSON and XML are used for packaging information in the Ajax model.

---

概念：

AJAX (Asynchronous JavaScript And XML):

异步的[[Tree/javascript\|javascript]]和[[Tree/XML\|XML]]

AJAX作用：

1. 与服务器进行数据交换：通过AJAX可以给服务器发送请求，并获取服务器响应的数据
2. 异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用校验，等等…

# Get Started

1. 编写AjaxServlet,并使用response输出字符串
2. 创建XMLHttpRequest对象：用于和服务器交换数据

```js
var xmlhttp;
if (window.XMLHttpRequest){
//code for IE7+,Firefox,Chrome,Opera,Safari
xmlhttp = new XMLHttpRequest();
} else { 
//code for IE6,IE5
xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}	
```
3. 向服务器发送请求
```js
xmlhttp.open("GET","url");
xmlhttp.send()  //发送请求
// 相当于get/post 
```
4. 获取服务器响应数据

```js
xmlhttp.onreadystatechange function(){
if(xmlhttp.readyState ==4 && xmlhttp.status ==200){
alert(xmlhttp.responseText);
```