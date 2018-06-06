---
title: 'ES6 中的Promise'
date: 2018-05-24 17:41:05
tags: ES6
---
js的执行环境是单线程，一次只能执行一个任务，如果任务多的话就得排队，只有等待前面的任务执行完毕才可以执行下一个任务。
这种单线程的优点是简单操作容易，但是也有缺点，任务过多，或者某个任务死掉的话会导致下面的任务‘阻塞’，导致用户体验很差。
一直以来js处理异步都是使用callback。
<!--more-->
callback的实现原理是当一个A函数运行了一定时间后去调用B函数。

```
function A(){

    setTimeout(function(){
        B()
    },5000)
}

function B(){
     setTimeout(function(){
        C()
    },5000)
}

function C(){
     setTimeout(function(){
        //....
    },5000)
}

```
这样虽然解决了回调问题，但是当回调嵌套过多时让代码非常繁琐。这种情况下我们一般叫做 ___(地狱回调)。
因此Promise规范被整理出来，来解决地狱回调问题。

##### Promise概念
Promise是异步编程的一种解决方案，它有三种状态。
pedding(进行中)
resolved(已完成)
rejected(已失败)
当Promise的状态由Pedding变为 resolved 或者 rejected 时 状态不可再次改变。

##### Promise用法
传统的回调函数写法
```
function a(){
    if(Math.random() > .5){
        successCallback("success")
    }else{
        failCallBack("fail")
    }
}

function successCallback(result){ //成功回调
    console.log(result)
}
function failCallBack(error){ //失败回调
   console.log(error)  
}

```
Promise的基本用法

```
var promise = new Promise(function(resolve,reject){
    if(success){
        resolve(a)
    }else{
        reject(err)
    }
})
//或者

function promise(){
    return new Promise(function(resolve,reject){
        if(success){
            resolve(a)
        }else{
            reject(err)
        }
    })
}
```
Promise对象一旦实例化就会立即执行

##### Promise 基本属性方法

1.Promise.resolve(); 执行成功
2.Promise.reject(); 执行失败
3.Promise.reac(); 意思是当Promise.race([a1,a2,a3])中有三个结果，当谁先执行完的时候先返回，返回最先执行完的那个结果。
4.Promise.all();  Promise.all可以将多个Promise实例包装成一个新的Promise实例。同时，成功和失败的返回值是不同的，成功的时候返回的是一个结果数组，而失败的时候则返回最先被reject失败状态的值。
5.Promise.prototype.then();链式操作
6.Promise.prototype.catch();异常处理

###### Promise.resolve()与Promise.reject()
```
var myPromises = new Promise(function(resolve,reject){
	setTimeout(function(){
		resolve("成功！")
	},5000)
})
myPromises.then(function(data){
	console.log(data)
})
//等待5秒输出 '成功'

var myPromises = new Promise(function(resolve,reject){
	if(Math.random() > 0.5){
		resolve('成功')
	}else{
		reject('失败')
	}
})
myPromises.then(function(data){
	console.log(data)
})
```
执行结果
![](http://ot2pck40x.bkt.clouddn.com/1528207485%281%29.jpg)

###### Promise.reac() 与 Promise.all()

```
var a1 = new Promise(function(){
    setTimeout(function(){
        console.log('a1')
    },2000)
})

var a2 = new Promise(function(){
    setTimeout(function(){
        console.log('a2')
    },1000)
})

var a3 = new Promise(function(){
    setTimeout(function(){
        console.log('a3')
    },4000)
})

var a4 = new Promise(function(){
    setTimeout(function(){
        console.log('a4')
    },5000)
})


Promise.all([a1,a2,a3,a4]).then(function(data){
    console.log(data)
})
```
Promise.all()执行结果
![](http://ot2pck40x.bkt.clouddn.com/1528208260%281%29.jpg)

```
var a1 = new Promise(function(){
    setTimeout(function(){
        console.log('a1')
    },2000)
})

var a2 = new Promise(function(){
    setTimeout(function(){
        console.log('a2')
    },1000)
})

var a3 = new Promise(function(){
    setTimeout(function(){
        console.log('a3')
    },3000)
})

var a4 = new Promise(function(){
    setTimeout(function(){
        console.log('a4')
    },3000)
})

Promise.race([a1,a2,a3,a4]).then(function(data){
    console.log(data)
})
```
执行结果![](http://ot2pck40x.bkt.clouddn.com/1528208356%281%29.jpg)

###### Promise.prototype.catch()
```
new Promise(function(resolve,reject){
    throw new Error('出现异常')
}).catch(function(err){
    console.log(err)
}) // 出现异常
```
##### 总结
 Promise的内容不止这些，以上为基本用法。

 参考链接：
 https://www.cnblogs.com/lvdabao/p/es6-promise-1.html
 https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises