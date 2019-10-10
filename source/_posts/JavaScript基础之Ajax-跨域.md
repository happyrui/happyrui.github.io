---
title: 'JavaScript基础之Ajax,跨域'
date: 2019-10-10 19:10:13
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---
#### Ajax

##### 原理

通过 XMLHttpRequest对象来向服务器发送异步请求，从服务器获得数据，然后用JS操作DOM，从而更新页面。

##### 编写步骤

+ 创建AJAX对象   XMLHttpRequest
+ 打开一个连接  open("GET",URL,asnyc)
+ 发送数据  send();
+ 事件处理函数，处理服务器的响应结果 onreadystatechange
<!-- more -->

##### 实现步骤

```js
//创建ajax对象
var request=null;
if(window.XMLHttpRequest){
	request = new XMLHttpRequest();
}else{
	request = new ActiveXObject("Microsoft.XMLHTTP");
}
//连接服务器
request.open("GET",url,true);
//发送请求
request.send();
//接收返回
request.onreadystatechange = function(){
	if(request.readyState==4 && request.status==200){
		fnSucc(request.responseText);
	}else{
		if(fnFail){
			fnFail();	
		}
	}
}
```

##### GET&POST
| --- | --- | --- | --- |
| --- | --- | --- | --- |
|get |将数据放在URL(网址)里面来提交  |安全性低 容量低,几K 获取数据 | 便于分享  如浏览帖子  可以缓存|
|post |将数据放在不是URL的地方   |    安全性一般 容量无限 上传数据 | 不便于分享 如用户注册|

可以使用jquery封装好的$.ajax去异步获取后台的数据

```js
$.ajax({
    type: "POST",
    url: url,
    data:{},//数据
    async: false,//同步
    dataType: 'json',
    success: function (data) {
        alert(JSON.stringify(data))
    },
    error: function (XMLHttpRequest, textStatus, errorThrown) {
        alert("报错");
    }
}); 
```
#### 跨域

[解决参考](https://segmentfault.com/a/1190000012469713/)

##### jsonp

在同源策略下，在某个服务器下的页面是无法获取到该服务器以外的数据的，但img、iframe、script等标签是个例外，这些标签可以通过src属性请求到其他服务器上的数据。

而JSONP就是通过script节点src调用跨域的请求。基于JSONP的实现原理,所以JSONP只能是“GET”请求,不能进行较为复杂的POST和其它请求

当我们通过JSONP模式请求跨域资源时，服务器返回给客户端一段javascript代码，这段javascript代码自动调用客户端回调函数

+ 原生JS实现：

```js
function createJs(sUrl){
    var oScript = document.createElement('script');
    oScript.type = 'text/javascript';
    oScript.src = sUrl;
    document.getElementsByTagName('head')[0].appendChild(oScript);
}
createJs('jsonp.js');
// src:
// http://www.baidu.com/json/?callback=box
// 回调函数
function box(json){
    alert(json.name);
}

// 服务器返回给客户端一段javascript代码
// jsonp.js
box({
   'name': 'test'
 });

```

+ Jquery实现 （使用封装的ajax）

```js
$.ajax({
	url:"http://localhost:8088/read.php",
	type:"GET",
	asnyc:false,
	dataType:"jsonp",
    jsonp: "callback",
    jsonpCallback:"box",
	success:function(data){
		alert(JSON.stringify(data.name)+" is "+JSON.stringify(data.age)+" years old");
	},
	error:function(){
		alert('fail');
	}
});
```

##### CORS解决跨域问题  跨域资源共享

CORS 需要浏览器和服务端同时支持的，对于兼容性来说主要是ie10+，其它现代浏览器都是支持的。

##### document.domain

##### window.name

##### HTML5中新引进的window.postMessage


ps:
![image](https://segmentfault.com/img/remote/1460000012469726?w=659&h=884)