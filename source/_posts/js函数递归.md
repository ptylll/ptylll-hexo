---
title: js函数递归
date: 2018-08-29 13:39:28
tags: js
categories: js
---

递归（英语：Recursion），又译为递回，在数学与计算机科学中，是指在函数的定义中使用函数自身的方法


#### 斐波那契数列
 (斐波那契数列是指这样的一个数列：1 1 2 3 5 8 13 21... 前两个数的和为下一个数的值)
 
 ```
  function fibona(n){
    if(n === 0 || n === 1){
      return 1
    }
    retrun n < 2 : n ? fibona(n - 1) + fibona(n - 2)
  }
  console.log(fibona(5)) // 5
  console.log(fibona(10)) //
 ```
<!--more-->
 #### 1到100累加
  传统方式实现
 ```
  function fn(){
    let sum = 0;
    let index = 1;
    while(index <= 100){
      sum += index
      index++
    }
    return sum
  }
  console.log(fn())//5050
 ```
  递归实现累加
  ```
  function fn(n){
    return n < 100 ? n + fn(n + 1) : n
  }
  console.log(fn(1))//5050
  ```

 #### 乘阶

乘阶传统实现方式
```
function fn(n){
  let sum = 1;
  let index = 1;
  if(n === 0){
    return 0
  }
  if(n === 1){
    return 1
  }
  while(index <= n){
    sum *= index
    index ++
  }
  return sum
}
console.log(fn(5))//1*2*3*4*5=120
```
递归方式实现
```
function fn(n){
  if(n === 0){
    return 0
  }
  if(n === 1){
    return 1
  }
  return n * fn(n - 1)
}
console.log(fn(5))//1*2*3*4*5=120
```
很显然递归减少了很多代码

##### 递归需要具备的条件

* 必须有一个明确的条件判断递归出口递归入口。
* 当递归边界条件满足时 递归返回。当递归条件不满足时 继续执行。

##### 递归特点

* 递归必须有出口条件，否则成为死循环。
* 递归的子问题与原始问题必须相同。


参考地址：
https://github.com/mqyqingfeng/Blog/issues/49
https://zh.wikipedia.org/wiki/%E9%80%92%E5%BD%92