---
title: localstorage sessionstorage cookie 使用场景以及区别
date: 2018-04-14 21:44:25
tags: js
---
localstorage sessionstorage cookie 使用场景以及区别

### localStorage

localStorage是HTML5新加入的特性，主要用来本地存储的，解决cookie存储空间不足问题(cookie中每条cookie的存储空间为4k),
localStorage中一般浏览器支持的是5M大小(不同的浏览器中localStorage会有所不同)。
主要用数据持久化，没有过期时间设置。
```
注意： 当浏览器进入隐私浏览模式时，会创建一个新的临时数据库来存储本地存储数据; 
当隐私浏览模式被关闭时，该数据库被清空并丢弃
```
<!--more-->
#### localStorage 语法示例

获取本地Storage对象
```
var data = localStorage.getItem('mydata');
```
增加一条数据
```
localStorage.setItem('mydata','data');
```
移除一条localStorage 
```
localStorage.removeItem('mydata');
```
### sessionStorage 
sessionStorage  里面的数据在页面会话结束时会被清除。页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。在新标签或窗口打开一个页面会初始化一个新的会话

#### sessionStorage 语法示例

保存数据到sessionStorage
```
sessionStorage.setItem('key', 'value');
```
从sessionStorage获取数据
```
var data = sessionStorage.getItem('key');
```
从sessionStorage删除保存的数据(某一项)
```
sessionStorage.removeItem('key');
```
从sessionStorage删除所有保存的数据(所有项)
```
sessionStorage.clear();
```

### cookie 

Cookie是由HTTP服务器设置的，保存在浏览器中，但HTTP协议是一种无状态协议，在数据交换完毕后，服务器端和客户端的链接就会关闭，每次交换数据都需要建立新的链接。
Cookie 与localStroage类似 是数据持久化的一种解决方案。

#### cookie 语法示例

读取cookie
```
function getCookie(key,value,option){
    if(typeof value === 'undefined'){
        var cookie = document.cookie.split(";");
        for(var i = 0 ,len =cookie.length; i<len;i++){
            var cookie = cookie[i].split("=");
            if(decodeURLComponent(cookie[0] === key)){
                return decodeURLComponent(cookie[1])
            }
        }
    }
    return null
}
```
存储Cookie
```
function setCookie(key,value,time){
    var date = new Date();
        date.setDate(date.getDate() + time );
        document.cookie= key + "=" + value +';expires=' + date;
            
}
```
删除指定的Cookie

```
function removeCookie(key){
    setCookie(key,'',-1)//原理是把Cookie保质期退回一天便可以删除
}
```

区别：
```
1.cookie在浏览器和服务器端来回传递数据，而localStorage和sessionStorage不会自动把数据
发送给服务器，仅会保存在本地。cookie会在浏览器请求头或者ajax请求头中发送cookie内容。
2.cookie 可以设置过期时间，sessionStorage是会话级别的浏览器关闭数据就会清除，
localStorage是永久性的，一旦存储除非手动清除，否则一直存在。
3.cookie 存储数据有限（4k左右），sessionStorage与localStorage大小在5M左右
浏览器不同大小不同）。
4.sessionStorage 不跨窗口，窗口关闭即消失。cookie，localStorage不受窗口限制。

```

参考链接：
https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage
https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage
https://www.cnblogs.com/zmj-blog/p/7120282.html