---
{"dg-publish":true,"permalink":"/tree/axios/","tags":["CS/web/AJAX"],"created":"2022-08-14T20:50:21.000+08:00","updated":"2023-08-27T03:04:22.888+08:00"}
---


# Introduction

Axios是一个基于`promise`网络请求库，作用于node.js和浏览器中。它是isomorphic的（即同套代码可以运行在浏览器和node.js中) 在服务端它使用原生node.js `http`模块，而在客户端（浏览端)则使用`XMLHttpRequests`

**特性**
- 从浏览器创建`XMLHttpRequests`
- 从node.js创建http请求
- 支持Promise API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRE

# Get Started

1. 引入axios的js文件
```html
<script src="js/axios-0.18.0.js"></script>
```
2. 使用axios发送请求，并获取响应结果
```js
axios({
	method:"get",
	url:"http://localhost:8080/ajax-demo1/aJAXDemo1?username=zhangsan"
)).then(function (resp){
	alert(resp.data);
})
```
```js
axios({
	method:"post",
	url:"http://localhost:8080/ajax-demo1/aJAXDemo1",
	data:"username=zhangsan"
}).then(function (resp){
	alert(resp.data);
````

---

为了方便起见，Axios已经为所有支持的请求方法提供了别名

```js
axios.get(url[,config])
axios.delete(url[,config])
axios.head(url[,config])
axios.options(url[,config])
axios.post(url[,data[,config]])
axios.put(url[,data[,config]])
axios.patch(url[,data[,config]])
```

- 发送get请求
```js
axios.get("url")
	.then(function(resp){
		alert(resp.data);
});
```

- 发送post请求
```js
axios.post("url","参数")
	.then(function(resp){
		alert(resp.data);
});
```

