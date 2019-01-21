---
layout: 'JavaScript高级程序设计阅读'
title: javascript数据类型
date: 2019-01-21 11:58:47
tags: JavaScript高级程序设计阅读
---

javascript有 5 种简单数据类型（也称为基本数据类型）：<font color="#f00">Undefined</font>、<font color="#f00">Null</font>、<font color="#f00">Boolean</font>、<font color="#f00">Number</font>和 <font color="#f00">String</font>。还有 1种复杂数据类型 <font color="#f00">Object</font>，Object 本质上是由一组无序的名值对组成的。javascript不支持任何创建自定义类型的机制，而所有值最终都将是上述 <font color="#f00">6</font> 种数据类型之一。
### 由于js是松散性类型的，所以需要提供一种检测类型的方法，typeof就是检查当前数值类型的操作符。
```
"undefined"——如果这个值未定义;

"boolean"——如果这个值是布尔值;

"string"——如果这个值是字符串;

"number"——如果这个值是数值;

"object"——如果这个值是对象或 null;

"function"——如果这个值是函数;

typeof null会返回"object"，因为特殊值 null 被认为是一个空的对象引用。
```
<!--more-->
#### Undefined

Undefined 类型只有一个值，undefined。当我们声明一个变量值未对其初始化时，这个变量值就为undefined.比如：
```
var a
console.log(a === undefined) //true
```

注意：
* undefined无法使用delete删除，也无法使用for/in枚举
* undefined不是常量，但是可以修改它的值
* 当我们获取一个不存在的值时会返回undefined
* undefined 不是js中的保留字，在非严格模式下可以重写undefined的值，严格模式下无法修改。
* 某些情况下为了防止undefined被修改可以通过viod 0 来代替undefined
  
#### Null

Null 类型是第二个只有一个值的数据类型，这个特殊的值是 null。从逻辑角度来看，null 值表
示一个空对象指针，而这也正是使用 typeof 操作符检测 null 值时会返回"object"的原因，如下面
的例子所示：

```
var a = null;
alert(typeof a); // "object"
```
##### 判断Null类型

错误示例：

```
var a ;
if(a == null){
  console.log('is null') //is null
}
```
当我们把undefined 赋值给a的时候发现 依然输出a 为null；
原因：

（如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 null 而不是其他值。这样
一来，只要直接检查 null 值就可以知道相应的变量是否已经保存了一个对象的引用。）
```
console.log(undefined == null) //true
```
所以我们判断一个数值是否为Null类型时，要排除undefined

正确示例:
```
var a = null
if(!a && typeof a !== 'undefined' && a != 0){
console.log('is null')
}
```
在线示例:
<script async src="//jsfiddle.net/ptylll/cabe2gyx/5/embed/"></script>