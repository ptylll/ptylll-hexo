---
title: ES6 class 的基本用法
date: 2018-04-04 14:14:20
categories: 
- ES6
tags: ES6
---

ES6 class 的基本用法

<!--more-->
js传统做法是当生成一个对象实例，需要定义构造函数，然后通过new的方式完成。

```js
  function Person(){
    this.name="张三";
    this.age = 15,
  }
  
  Person.prtototype.getNames = function(){
    console.log('name：'+this.name)
  }
  
  var a = new Person();
      a.getNames();//张三
```

ES6 写法

```js
class Person{
  constructor(){
    this.name="张三",
    this.age = 15
  }
  getNames(){
      console.log('name:'+this.name)
  }
}

var a = new Person();
    a.getNames();
```

在构造函数prototype上面的的属性都是存在的；

```js
class Person{
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
//两者相等

```

生成类的实例对象的写法，与 ES5 完全一样，也是使用new命令。前面说过，如果忘记加上new，像函数那样调用Class，将会报错

```js
  class Person{
    
  }
  var a = Person();//报错
  var a = new Person();//正确
  
```

### 类表达式

```js
const People = class Person{
   getNames(){
    return Person.name
  }
}
var a = new People();
    a.getNames(); // Person
var b = new Person();
    b.getNames();//not defined
```

Person只在 Class 的内部代码可用，指代当前类

### 立即执行函数

```js
 const People = new class{
     constructor(name) {
        this.name = name
     }
     
     getNames(){
        console.log(this.name)
     }
 }('张三');
 
  People.getNames();//张三
 ```

### class 存值取值get() set()

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'

```

### class 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class People{
 constructor() {
    // ...
  }
  static  getName(){
    console.log('张三')
  }
};
People.getName();//张三
var a = new People();
    a.getName();// is not a function
    
```
###### 父类的静态方法，可以被子类继承。

```js

class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'

```

###### 静态方法也是可以从super对象上调用的。

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}

Bar.classMethod() // "hello, too"

```
### extends继承
 
 ``` js
 class Parent{
    contructor(name,age){
        class Parent {
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    speakSometing(){
        console.log("I can speek chinese");
    }
}
class Child extends Parent {//定义子类，继承父类
    coding(){
        console.log("coding javascript");
    }
}
var c = new Child();
//可以调用父类的方法
c.speakSometing(); // I can speek chinese

```
在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。这是因为子类实例的构建，是基于对父类实例加工，只有super方法才能返回父类实例。

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```

### Object.getPrototypeOf()

Object.getPrototypeOf方法可以用来从子类上获取父类。
```js
Object.getPrototypeOf(ColorPoint) === Point

```
可以使用这个方法判断，一个类是否继承了另一个类。

### super 

super这个关键字，既可以当作函数使用，也可以当作对象使用。

当函数使用时

```js
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```

super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

```js
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B();
```

### mixin多继承

mixin就是把一个对象的方法和属性拷贝到另一个对象上，注意这个继承还是有区别的。js是一种只支持单继承的语言，毕竟一个对象只有一个原型，如果想实现多继承，那就简单暴力的把需要继承的父类的所有属性都拷贝到子类上，就是使用mixin啦。

### demo 
简单的mixin 拷贝某些方法

```js
  function extend(destClass, srcClass, methods) {
        var srcProto  = srcClass.prototype;
        var destProto = destClass.prototype ;   
        for (var i=0; i<methods.length; i++) {
            var method = methods[i];
            if (!destProto[method]) {
                destProto[method] = srcProto[method];
            }
        }
    }
    function Book() {}
    Book.prototype.getName = function() {};
    Book.prototype.setName  = function() {};

    function JS() {}

    extend(JS, Book, ['getName']);
    var js = new JS();
    console.log(js);
```
![](http://ot2pck40x.bkt.clouddn.com/L%5D5B30_IORO%5DC%7BDXR~2%2931Q.png)

mixin 多继承
```js
  function extend(destClass) {
    var classes = Array.prototype.slice.call(arguments, 1);
    for (var i=0; i<classes.length; i++) {
        var srcClass = classes[i];
        var srcProto  = srcClass.prototype;
        var destProto = destClass.prototype;  
        for (var method in srcProto) {
            if (!destProto[method]) {
                destProto[method] = srcProto[method];
            }
        }      
    }
}
function Book() {}
Book.prototype.getName = function() {};
Book.prototype.setName  = function() {};

function Tech(){}
Tech.prototype.showTech = function(){};

function JS() {}

extend(JS, Book, Tech);
var js = new JS();
console.log(js);
```
![](http://ot2pck40x.bkt.clouddn.com/~SZU%28$W1%5B~NRN8_KY6S311V.png)


参考：http://es6.ruanyifeng.com/#docs/class<br/>
https://segmentfault.com/a/1190000007537173<br/>
https://blog.csdn.net/qiqingjin/article/details/51762435
