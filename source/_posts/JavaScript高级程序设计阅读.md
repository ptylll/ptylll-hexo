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

#### Boolean

Boolean 类型只有两个值true/false。虽然Boolean只有两个值但是js中是所以的值都是与之等价的，如果要将其中一个值转化为布尔类型可以直接使用Boolean()方法。
```
Boolean(123) //true
Boolean(0)//false
Boolean(null)//false
Boolean(undefined)//false
Boolean('')//false
```

转化规则：
```
Boolean 类型 false  true
String 类型除 ''(空字符串)其余都为true
Number 类型除NaN 0 其余都为true
Object 类型除 Null 都为true
Undefined 类型都为false
```

#### Number

Number 类型使用 IEEE754 格式来表示整数和浮点数值。
默认情况下为十进制.
```
var a = 123
```
除十进制外整数还可以通过八进制十六进制表示。

注意:<font color='#f00'>八进制第一位必须为0，八进制数字序列（0~7）如果超出了这个范围，前面的零将忽略掉数值当作十进制解析。</font>
示例：
```
console.log(parseInt(0129)) //解析为129
console.log(parseInt(0127) //87 
```
注意:<font  color='#f00'>很多情况下我们转换的大部分都为十进制，把10当作第二个参数很有必要。</font>
```
console.log(parseInt(210.258,10,10))//210
```
##### Number类型判断
Number类型判断一般使用typeof 判断返回值是否为Number
```
let a = 10
console.log(typeof a === 'number') // true
```
#### String类型
String 类型用于表示由零或多个 16 位 Unicode 字符组成的字符序列，即字符串。字符串可以由双引号（"）或单引号（'）表示。
因此不管是双引号还是单引号都是正确的写法。
```
let a = '123'
let b = "123"
```
但是符号前后是需要匹配的，前后符号不匹配汇报语法错误。

```
let a = "123' //Uncaught SyntaxError: Invalid or unexpected token
```
##### 特点

ECMAscript中 字符串一旦创建就不能，如果需要修改某个变量的字符串。首先会销毁原始的字符串，再用新得字符串来替换填充此变量。
示例：
```
let a = '123'
a = a + '456'
//上面默认a的值为123,当我们给它添加一个456字符串时，先创建一个新的字符串长度为6,把123 456填充到新的字符串中，再销毁原始a的值把新的值赋给a。
```
##### 字符串转换
字符串转换一般通过toString()方法。
默认情况下toString()为按照10进制转化。

示例：
```
let a = 1000
console.log(a.toString()) // '1000'
console.log(a.toString(8))// '1750'
console.log(a.toString(16))//3e8
console.log(a.toString(32))//v8
```

当我们再转化为字符串时要注意：
```
1.toString()会报错
```
![](https://image-static.segmentfault.com/300/111/3001112184-5c3d358b09fb6_articlex)
![](https://image-static.segmentfault.com/236/763/2367638522-5c3d362581ca0_articlex)
javascript引擎在解释代码时对于“1.toString()”认为“.”是浮点符号，但因小数点后面的字符是非法的，所以报语法错误；
而后面的“1..toString()和1.2.toStirng()”写法，javascript引擎认为第一个“.”小数点，的二个为属性访问语法，所以都能正确解释执行；
对于“(1).toStirng()”的写法，用“()”排除了“.”被视为小数点的语法解释，所以这种写法能够被解释执行；

纯小数的小数点后面有连续6或6个以上的“0”时，小数将用e表示法进行输出

![](https://image-static.segmentfault.com/552/008/55200867-5c3d36c66a995_articlex)

当我们不知道要转换的值是不是 null 或 undefined 的情况下，还可以使用转型函数String()，这个函数能够将任何类型的值转换为字符串。String()函数遵循下列转换规则：

* 如果值有 toString()方法，则调用该方法（没有参数）并返回相应的结果；
* 如果值是 null，则返回"null"；
* 如果值是 undefined，则返回"undefined"。

#### Object类型
 Object对象其实就是一组数据和功能的集合。对象可以通过执行 new 操作符后跟要创建
的对象类型的名称来创建。而创建 Object 类型的实例并为其添加属性和（或）方法，就可以创建自定
义对象。

创建Object对象有两种方式：

第一种通过new Object构造函数;
```
let a = new Object()
a.name = '123'
a.age = 23
```
第二种通过对象字面量表达式创建：
```
let a = {
  name:'123',
  age:23
}
```
总结：以上就是js中基本数据类型。（除ES6 中的Symbol类型）在使用的同时要注意基本的细节，不然带来莫名的bug。