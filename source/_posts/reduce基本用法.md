---
title: reduce基本用法
date: 2019-07-03 10:11:49
tags: js
categories: 
- js
---

`reduce()` 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。

#### 语法

```js
arr.reduce(function(total,currentValue,currentIndex,arr)[ initialValue])
```
`reduce`函数接收4个参数：
1. total (acc) (累计器)
2. currentValue Value (cur) (当前值)
3. currentIndex Index (idx) (当前索引)
4. initialValue (src) (源数组)

回调函数第一次执行时并且无initialValue时，prev的值为arr的第一项，cur的值为arr的第二项，index索引从1开始；
若有initialValue时，prev的值为initialValue, cur的值为arr的第一项，index索引从0开始；
initialValue设置prev的初始类型和初始值也就是叠加结果的类型。

<!--more-->

#### 用法

##### 数组求和
```js
let arr = [4,5,6]
let sum = arr.reduce((total,curr)=>total + curr) //15
```
##### 对象数字求和
```js
let arr = [
  {
    id:0,
    num:1
  },
  {
    id:1,
    num:10
  },
  {
    id:2,
    num:2
  },
  {
    id:3,
    num:3
  },
]
let sum = arr.reduce((total,curr)=>total + curr.num,0)//16
```
##### 数组最大值

```js
let arrMax = [1,2,3,4,5,6,7,8,9]
let max = arrMax.reduce((pre,curr)=>{
  return pre[curr] > curr ? pre[curr] : curr
})//9
```
##### 求字符串中字母出现的次数
```js
let str = 'qwertyyuiqweavdgwerwr'
let strObj = str.split('').reduce((pre,curr)=>{
    pre[curr] ? pre[curr]++ : pre[curr] = 1
    return pre
},{})// {a: 1, d: 1, e: 3, g: 1, i: 1, q: 2, r: 3, t: 1, u: 1, v: 1, w: 4, y: 2}

```
##### 数组除重

```js
let array = [1,2,3,4,5,6,7,8,9,1,1,3,45,4,,4,5]
let removerRepetitionItem = array.reduce((pre,curr)=>{
    if(pre.indexOf(curr) < 0) pre.push(curr)
    return pre
},[])//[1, 2, 3, 4, 5, 6, 7, 8, 9, 45]

```

##### 多维数组拍平

```js
let multiArray =[[12,3,45],[4,56,7],[9,9,9]]
let arrayList = multiArray.reduce((pre,curr)=>pre.concat(curr),[])//[12, 3, 45, 4, 56, 7, 9, 9, 9]
```
代码在线地址：
https://runkit.com/pty/5d1c14f54eb6d40014e27c7a
参考链接：
https://juejin.im/post/5b4d35406fb9a04fd55ac064
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce