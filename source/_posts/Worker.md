---
layout: 'h5新特性web'
title: Worker
date: 2018-05-07 20:26:52
tags: H5
categories: H5
---
javascript是单线程,一次只能执行一个任务。当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。
这就会导致影响页面性能问题。
web Worker是运行在后台的js脚本，不影响页面性能。
<!--more-->
##### 兼容性
所有主流浏览器均支持 web worker，除了 Internet Explorer。
##### Worker可操作的对象包括：

1. 一个浏览器对象，只包含4个对象：appName, appVersion, userAgent,2.platform
3. 一个location对象（和window里的一样，但是里面所有的属性是只读的）
4. 一个self对象指向全局Web Worker线程对象
5. 一个importScripts()方法使Web Worker能够加载外部js文件
6. 所有的ECMAScript对象
7. XMLHttpRequest构造器
8. setTimeout()和setInterval()方法

##### Worker兼容性检测
```
if(window.Worker){
    ...
}
```
##### Worker创建
Worker 创建非常简单
```
var worker = new Worker('worker.js');
```

##### Worker发送接收消息

Worker 通过 postMessage()和onmessage()来发送接收消息。

```
worker.js
//发送消息 
postMessage('hello world');

```
```
//index.html
 var worker = new Worker('worker.js');
    worker.onmessage=function(event){//接收消息
        console.log(event.data)// hello world
    }
```
##### Worker终止 
创建 web worker 对象后，它会继续监听消息（即使在外部脚本完成之后）直到其被终止为止。
```
worker.terminate();
```
而在worker线程中，workers 也可以调用自己的 close  方法进行关闭(关闭当前线程): 
```
close();
```

##### 处理错误
onerror 处理错误事件 
```
worker.onerror = function(e){
    console.log('erro:'+ e.message);
        worker.terminate();
}
```
##### importScript
importScript是以阻塞方法加载js的，只有所有文件加载完成之后接下来的脚本才能继续运行。

```
importScripts(worker1.js,worker2.js)
worker.onmessage = function(event){
    console.log(event.data)
}
```
Worker的优点是创建多线程执行js，当然也有一定的局限性：
. 不能跨域
. worker内的代码不能访问DOM

##### Worker 实现简单例子

```
//index
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>worker</title>
        <script>
            function init(){
                //创建一个Worker对象，并向它传递将在新线程中执行的脚本url
                var worker = new Worker('worker.js');
                    worker.onmessage = function(event){
                        document.getElementById('result').innerHTML+=event.data+"<br/>" ;
                    };
            };
        </script>
    </head>
    <body onload = "init()">
        <div id="result"></div>
    </body>
</html>

```

```
//worker.js
function timedCount(){
    for(var j = 0, sum = 0 ;j < 100 ; j++){
        for(var i = 0; i < 1000000; i++ ){
            sum += i;
        }
    }
    postMessage(sum);
}

postMessage('开始时间'+new Date());

timedCount();

postMessage('结束时间'+new Date());

```
注意：chrome不支持本地Worker执行 Worker 必须放到服务器上执行
![](http://ot2pck40x.bkt.clouddn.com/4MBC%602R14~7K%7DK%25VV%252PRL8.png)
##### 总结
Web Worker可以在后台执行脚本，而不会阻塞页面交互。执行纯数据的与浏览器UI没关系的长运行脚本。请求比较大的JSON,加载一个JS进行大量的复杂计算而不挂起主进程。
参考链接：
https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers
https://blog.csdn.net/dojotoolkit/article/details/25030289