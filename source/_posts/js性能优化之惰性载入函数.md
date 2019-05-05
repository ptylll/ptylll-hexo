---
title: js性能优化之惰性载入函数
date: 2019-04-30 17:45:39
tags: 前端性能
---

由于浏览器之前存在着差异，我们在写js是为了兼容不同浏览器，需要通过if else分支引导浏览器执行正确的代码。

比如我们兼容不同浏览器的事件绑定。

```js
function addEvent (type, element, fun) {
    if (element.addEventListener) {
      element.addEventListener(type, fun, false);
    }else if(element.attachEvent){
      element.attachEvent('on' + type, fun);
    }else{
      element['on' + type] = fun;
    }
}
```
<!--more-->
以上写法有个问题，当我们每次调用这个事件绑定函数时都需要走下if else判断。
修改以上写法：

```js
var addEvent = (function(){
  if (element.addEventListener) {
    return function(type, element, fun){
      element.addEventListener(type, fun, false)
    }
  }else if(element.attachEvent){
    return function(type, element, fun){
      element.attachEvent('on' + type, fun);
    }
  }else{
   return function(type, element, fun){
    element['on' + type] = fun;
   }
  }
})()
```
例子中使用的技巧是创建一个匿名、自执行的函数，用以确定应该使用哪一个函数实现。实际
的逻辑都一样。不一样的地方就是第一行代码（使用 var 定义函数）、新增了自执行的匿名函数，另外
每个分支都返回正确的函数定义，以便立即将其赋值给 addEvent()。

惰性载入函数在执行分支时会消耗一点性能，一旦执行完成后面就不需要再重复执行不必要的分支。

使用场景：
1 应用频繁，如果只用一次，是体现不出它的优点出来的，用的次数越多，越能体现这种模式的优势所在；

2 固定不变，一次判定，在固定的应用环境中不会发生改变；