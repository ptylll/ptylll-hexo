---
title: ES6异步之 async-await
date: 2018-08-08 23:00:01
tags: es6
categories: ES6
---

async 为 generator 的语法糖，它的作用与字面上一个意思异步。

#### 基本语法

async 函数返回一个 Promise 对象，可以用 then()方法添加回调函数。当函数执行过程之遇到 await 先返回，等异步完成再执行后面的语句。

```
 async function getMath(){
    let random = Math.random();
    console.log(random)
 }

 getMath()
 // 0.5456369101126857
 // Promise {<resolved>: undefined}__proto__: Promise[[PromiseStatus]]: "resolved"[[PromiseValue]]: undefined
```

<!--more-->

#### async 声明方式

```
//函数声明
async function foo(){

}

//函数表达式声明
const foo = async function(){

}

//对象声明
let obj = {
    async foo(){

    }
}

// es6 class声明
class oof {
    constructor(){
        super()
    }
    async getName(name){
        return name
    }
};

//箭头函数声明
const oof = async ()=>{

};
```

#### async 处理错误

##### 1.0 使用 try catch 捕获

```
 async foo (){
    try{
        await returnPromise()
    }.catch(error){
        console.log(error)
    }
 }
```

##### 2.0 异步操作添加 catch 回调函数

```
 async foo (){
    await returnPromise().catch(error => {
        console.log(error)
    })
 }
```

#### 实例

```
 async foo(){
    return await 12345678
 }

 foo().then(res=>{console.log(res)}) // 12345678
```

#### 注意事项

1、await 必须在 async 里面

2、async 作用于函数

3、async 返回一个 promise 对象

参考链接：
https://segmentfault.com/a/1190000008677697
http://es6.ruanyifeng.com/#docs/async
