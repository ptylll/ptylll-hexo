---
title: fetch与ajax的区别
date: 2018-04-09 21:33:47
categories: 
- js
tags: js
---
 ##### 一. Fetch是 window 下面的一个方法
 ###### Fetch写法：
 
 ```js
    fetch('url'，{
        methods:'get'
    })
    .then(function(res){
        return //执行成功, 第一步
    })
    .then(function(){
        return // 执行成功第二步
    })
    .catch(function(err){
        //异常报错
    })

 ```

<!--more-->

######  Fetch 常见处理:

HTML

```js
fetch('/user.html')
.then(function(res){
    return res.text();
})
.then(function(body){
    document.body.innerHTML = body;
})
```

JSON 

```js
    fetch('/users.json')
    .then(function(response) {
        return response.json()
    }).then(function(json) {
        console.log('parsed json', json)
    }).catch(function(ex) {
        console.log('parsing failed', ex)
    })

```

Post form

```js

    var form = document.querySelector('form');

    fetch('/user',{
        method:'POST',
        headers:{
            'Content-Type':'application/json'
        },
        body:JSON.stringify({
            name:'fetch',
            login:'fetct'
        })
    })

```
File upload

```js
var input = document.querySelector('input[type="file"]')

var data = new FormData()
data.append('file', input.files[0])
data.append('user', 'hubot')

fetch('/avatars', {
  method: 'POST',
  body: data
})
```
....
等等

##### 二.Ajax

###### 定义：

    AJAX全称“Asynchronous JavaScript and XML”（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。本质是使用XMLHttpRequest 来请求数据。

###### 举个栗子
```js
	function ajax(options){
			options = options || {};
			options.type = (options.type || "get").topUpperCase();
			options.dataType = options.dataType || "json";
		var params = formatParams(options.data);

		if(window.XMLHttpRequest){//创建xhr
			var xhr = new XMLHttpRequest();
		}else{
			var xhr = ActiveXObject('Microsoft.XMLHTTP');
		}
	}

	//接收
	xhr.onreadystatechange=function(){
		if(xhr,readyState == 4){
			var status = xhr.status;
			if(status >= 200 && options.success(xhr.responseText,xhr.responseXML)){
				options.success && options.success(xhr.responseText,xhr.responseXML);
			}else{
				options.fail && options.fail(status);
			}
		}
	}
	//连接发送
	if(options.type =="GET"){
		xhr.open("GET",option.url+"?"+params,true);
		xhr.send(null);
	}else if(options.type =="POST"){
		xhr.open("POST",options.url,true);
		xhr.setRequestHeader("Content-Type","application/x-www-form-urllencoded");
		xhr.send(params);
	}
	//格式化参数
	function formatParams(data){
		var arr=[];
		for (var name in data) {
			arr.push(encodeURLCompontent(name)+"="+encodeURLCompontent(data[name]));
		}
		arr.push(("v="+Math.random()).replace(".",""));
		return arr.join("&");
	}

    ajax({
        url: "./TestXHR.aspx",              //请求地址
        type: "POST",                       //请求方式
        data: { name: "super", age: 20 },        //请求参数
        dataType: "json",
        success: function (response, xml) {
            // 此处放成功后执行的代码
        },
        fail: function (status) {
            // 此处放失败后执行的代码
        }
	});
```
#### 总结

1.从 fetch()返回的 Promise 将不会拒绝HTTP错误状态, 即使响应是一个 HTTP 404 或 500。相反，它会正常解决 (其中ok状态设置为false), 并且仅在网络故障时或任何阻止请求完成时，它才会拒绝
2.默认情况下， 如果站点依赖维护用户会话（发送cookie，必须设置credentials init选项），  fetch 则不会发送或接收来自服务器的任何cookie，导致未经身份 验证的请求。
3.在语法上fetch 比ajax简洁了许多。
4.兼容上目前fetch 兼容不是太好 使用时需要引入 polyfill ：GitHub - github/fetch: A window.fetch JavaScript polyfill.

在默认情况下 fetch不会接受或者发送cookies，如果需要只能手动设置 credentials: 'include'

```js
fetct('/api',{
    credentials: 'include'
})
.then(function(res){
    return // ...
})
```
参考链接：
https://github.com/github/fetch
https://segmentfault.com/a/1190000003810652
https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/fetch