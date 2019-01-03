---
title: is-odd is-number 源码解读
date: 2019-01-03 14:22:41
tags: js
---

is-number源码：
```
module.exports = function(num) {
  if (typeof num === 'number') {
    return num - num === 0;
  }
  if (typeof num === 'string' && num.trim() !== '') {
    return Number.isFinite ? Number.isFinite(+num) : isFinite(+num);
  }
  return false;
};

```

<!--more-->

Number.isFinite()是ES6提供，检查数值是否为有限即数值是否为 Infinity
示例：
```
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false
```

is-odd 源码：
```
const isNumber = require('is-number');

module.exports = function isOdd(value) {
  const n = Math.abs(value);
  if (!isNumber(n)) {
    throw new TypeError('expected a number');
  }
  if (!Number.isInteger(n)) {
    throw new Error('expected an integer');
  }
  if (!Number.isSafeInteger(n)) {
    throw new Error('value exceeds maximum safe integer');
  }
  return (n % 2) === 1;
};
```

#### Math.abs()

```
Math.abs() 函数返回指定数字 “x“ 的绝对值
当传入一个非数字形式的字符串或者 undefined/empty 变量，将返回 NaN。传入 null 将返回 0。
```
示例：
```

Math.abs('-1231')// 1231
Math.abs('-1231a')//NaN
Math.abs()//NaN
Math.abs(undefined)//NaN
Math.abs(null) // 0
```
### Number.isInteger()用来判断一个数值是否为整数

JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。
由于 JavaScript 采用 IEEE 754 标准，数值存储为64位双精度格式，数值精度最多可以达到 53 个二进制位（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃，这种情况下，Number.isInteger可能会误判。


```
Number.isInteger(25.0)//true
Number.isInteger(25) //true
Number.isInteger(3.0000000000000000000000000000000000000004)//ture
```
#### Number.isSafeInteger()
JavaScript 能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。
Number.isSafeInteger()是用来判断一个整数是否落在这个范围之内。

```
Number.isSafeInteger('a') // false
Number.isSafeInteger(null) // false
Number.isSafeInteger(NaN) // false
Number.isSafeInteger(Infinity) // false
Number.isSafeInteger(-Infinity) // false

Number.isSafeInteger(3) // true
Number.isSafeInteger(1.2) // false
Number.isSafeInteger(9007199254740990) // true
Number.isSafeInteger(9007199254740992) // false

Number.isSafeInteger(Number.MIN_SAFE_INTEGER - 1) // false
Number.isSafeInteger(Number.MIN_SAFE_INTEGER) // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER) // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1) // false
```
#### ES5实现Number.isSafeInteger()方法

```
function isSafeInteger(val){
	return (typeof val === 'number' && Math.round(val) === val && Number.MIN_SAFE_INTEGER <= val && Number.MAX_SAFE_INTEGER >= val)
}
```
总结： 我们在判断数据类型时不仅仅要考虑到正常的数值，边界值也需要考虑进去这样才能保证程序的健壮性。