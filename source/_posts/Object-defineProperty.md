---
title: Object.defineProperty
date: 2018-06-14 19:38:25
tags: js
---

js 对象是由一个或者多个键值对组合而成的集合，每个属性对应任意类型的值。
声明一个对象方式

```
let obj = {}
//或者
let obj = new Object()
```

在对象上添加属性：

```
let obj ={}
    obj.name='李四';
    obj.age = 18;
    obj.msg = function(){
    	return this.name + this.age
    }
  console.log(obj.msg())  //李四 18
```

修改属性：

```
let obj ={}
    obj.name='李四';
    obj.age = 18;
    obj.msg = function(){
    	return this.name + this.age
    }
  console.log(obj.msg())  
  obj.name='张三';//修改属性
  console.log(obj.msg()) //张三 18
```

除了以上添加修改属性之外，Object.defineProperty()也可以修改添加原有的属性。

<!--more-->

#### 语法

Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
Object.defineProperty(obj,prop,desc);

```
obj:
要修改或者定义属性的对象

prop:
要修改或者定义的属性名称

desc:
要被定义或者修改的属性的描述
```

#### 属性描述

描述属性值主要有

```
configurable：
当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。

enumerable：
当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。（枚举可以使用for...in,Object.keys()循环出）
value：
该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
writable：
当且仅当该属性的writable为true时，value才能被赋值运算符改变。默认为 false
```

示例：

```
let obj ={}
    obj.name='李四';
    obj.age = 18;
    obj.msg = function(){
    	return this.name + this.age
    }
  console.log(obj.msg())  
  obj.name='张三';
  console.log(obj.msg())

  Object.defineProperty(obj,'sex',{//添加属性
  	value:'男',
    configurable:true
  })
  console.log(obj)

  obj.sex ='女';
  console.log(obj)//输出{name: "张三", age: 18, msg: ƒ, sex: "男"} 默认情况writable为false value是不会改变。

//添加writable属性
let obj ={}
    obj.name='李四';
    obj.age = 18;
    obj.msg = function(){
    	return this.name + this.age
    }
  console.log(obj.msg())  
  obj.name='张三';
  console.log(obj.msg())

  Object.defineProperty(obj,'sex',{//添加属性
  	value:'男',
    configurable:true
  })
  console.log(obj)

  obj.sex ='女';//直接修改属性 无效
  console.log(obj)


  Object.defineProperty(obj,'sex',{ //
  	value:'男',
    configurable:true,
    writable:true,
    enumerable:false
  })
  obj.sex = '女';
  console.log(obj) //{name: "张三", age: 18, msg: ƒ, sex: "女"}

//enumerable
for(let item in obj){
  	console.log(item) // name age msg
}

Object.defineProperty(obj,'sex',{ //
  	value:'男',
    configurable:true,
    writable:true,
    enumerable:true
})
for(let item in obj){
  	console.log(item) // name age msg sex
}

//configurable 是否delete可以删除

Object.defineProperty(obj,'sex',{ //可删
  	value:'男',
    configurable:true,
    writable:true,
    enumerable:true
})
console.log(obj) // name age msg

Object.defineProperty(obj,'sex',{ // 不可删
  	value:'男',
    configurable:false,
    writable:true,
    enumerable:true
})
console.log(obj) // name age msg sex
```

#### set get 存储器

```
get

一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。该方法返回值被用作属性值。默认为 undefined。

set
一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认为 undefined。
```

```
注意：get/set不能与 value,writeable同时使用。如果一个描述符同时有(value或writable)和(get或set)关键字，将会产生一个异常。
```

示例：

```
let obj = {}
obj.name = '李四';
obj.age = 18;
obj.msg = function() {
  return this.name + this.age
}
console.log(obj.msg())
obj.name = '张三';
console.log(obj.msg())

Object.defineProperty(obj, 'sex', { //添加属性
  value: '男',
  configurable: true
})
console.log(obj)

obj.sex = '女'; //直接修改属性 无效
console.log(obj)


Object.defineProperty(obj, 'sex', { //
  value: '男',
  configurable: true,
  writable: true,
  enumerable: false
})
obj.sex = '女';
console.log(obj) //{name: "张三", age: 18, msg: ƒ, sex: "女"}

for (let item in obj) {
  console.log(item)
}

Object.defineProperty(obj, 'sex', { //
  value: '男',
  configurable: true,
  writable: true,
  enumerable: true
})
for (let item in obj) {
  console.log(item) // name age msg sex
}
// configurable
delete obj.sex
console.log(obj)

Object.defineProperty(obj,'sex',{ // 不可删
  	value:'男',
    configurable:false,
    writable:true,
    enumerable:true
})
console.log(obj) // name age msg sex

let thisGrade = 3;
Object.defineProperty(obj,'grade',{
	get:function(){
  	return thisGrade
  },
  set:function(value){
   thisGrade	= value
  }
})
console.log(obj.grade)//3
obj.grade = 10;
console.log(obj.grade)//10
```
在线代码:
<script async src="//jsfiddle.net/ptylll/nyqkrzvm/embed/js,html/"></script>

总结：
Object.defineProperty()的用法远远不止这些，vue.js是通过它实现双向绑定的，俗称属性拦截器。

参考链接：
https://segmentfault.com/a/1190000007434923
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty
