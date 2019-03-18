---
title: js常用函数封装
date: 2018-05-20 21:25:06
tags: js
categories: js
---

### 手机号码验证

```
    function isPone(tel){
        var reg =  /^[1][3,4,5,7,8][0-9]{9}$/;  ;
    if (!reg.test(tel)) {
        return false;
   } else {
        return true;
   }
 }
```

<!--more-->

### 电话号验证

```
    function isTel(tel) {
        var reg = /^(([0\+]\d{2,3}-)?(0\d{2,3})-)(\d{7,8})(-(\d{3,}))?$/;
        if (!reg.test(tel)) {
            return false;
        } else {
        return true;
    }
 },
```

### 除去空格

```
// type 0 除去全部空格 1 除去前后空格 2 出去前空格 3 出去后空格
function trim(str,type){
    switch(type){
        case 0:
            return str.replace(/\s+/g,"");
        case 1:
            return str.replace(/(^\s*)|(\s*$)/g, "");
        case 2:
            return str.replace(/(^\s*)/g, "");
        case 3:
            return str.replace(/(\s*$)/g, "");
        default:
            return str;
    }
}
```

### 首字母大小写转换

```
// 0大写 1小写  2全部大写 3全部小写
function strCase(str,type){
    switch(type){
        case 0:
            return str.substring(0,1).toUpperCase()+str.substring(1);
        case 1:
            return str.substring(0,1).toLowerCase()+str.substring(1);
        case 2:
            return str.toUpperCase();
        case 3:
            return str.toLowerCase();
        default:
            return str
    }
}
```

### 生成随机码

```
// count 2~36
function randomWord(count){
    return Math.random().toString(count)+substring(2)
}
```

### 判断数组是否重复

```
//true 为重复 false 不重复
function isArrRepeat(arr){
  var arr = arr.sort();
  for(var i = 0,len = arr.length; i < len;i++){
    if(arr[i] == arr[i+1]){
      return true
    }else{
      return false
    }
  }
}
```

### cookie 增加 删除 获取 Cookie

```
var handleCookie = {
  addCookie:function(name,value,time){ // 存储cookie
    var str = name +"="+ escape(value);
    if(time > 0){
      var date = new Date();
      var ms = time * 3600 * 1000;
      date.setTime(date.getTime() + ms);
      str += "; expires=" + date.toGMTString();
    }
    document.cookie = str
  },
  getCookie:function(name){ // 获取cookie
    var str = document.cookie;
    if (str == "") {
        return "false"; //没有找到Cookie值
    }
    var arrStr = document.cookie.split("; ");
    for (var i = 0; i < arrStr.length; i++) {
        var temp = arrStr[i].split("=");
        if (temp[0] == name)
            return unescape(temp[1]);
    }
  },
  CleanCookie:function(name){ //删除cookie
    var date = new Date();
    date.setTime(date.getTime() - 10000);
    document.cookie = name + "=a; expires=" + date.toGMTString();
  }
}
```

### 数据类型判断

```
  function isObj(val){
    var type = typeof val;
    var getType = Object.prototype.toString.call(val);
    if(type === 'object'){
      switch(getType){
        case '[object Array]':
          return 'Array';
        case  '[object Function]':
          return 'Function';
        case '[object Null]':
          return 'null';
        case '[object Undefined]':
          return 'undefined'
        default:
          return type
      }
    }
    return type
  }
```

### 深拷贝

```
function deepClone(obj) {
  let objClone = Array.isArray(obj) ? [] : {}
  if (obj && typeof obj === 'object') {
    for (var key in obj) {
      if(obj.hasOwnProperty(key)) { //判断ojb子元素是否为对象，如果是，递归复制
        if (obj[key] && typeof obj[key] === "object") {
          objClone[key] = deepClone(obj[key]);
        } else { //如果不是，简单复制
          objClone[key] = obj[key];
        }
      }
    }
  }
  return objClone
}
```

### 阻止冒泡或捕获

```
 function stopBubble(e){
   e = e || window.event;
   if(e.stopPropagetion){//W3C
     e.stopPropagetion()
   }else{//IE
     e.cancelBubble = true;
   }
 }
```

#### 阻止默认行为

```
function stopdefault(e){
  e = e || window.e
  if(e.preventDefault){
    e.preventDefault()
  }else{ // IE
    window.event.returnValue = false;
  }
}
```

（dom 事件流执行的机制是先捕获再冒泡）
"DOM2 级事件”规定的事件流包含三个阶段：事件捕获阶段，处于目标阶段和事件冒泡阶段。首先发生的是事件捕获，然后是实际的目标接收到事件，最后阶段是冒泡阶段。

#### DOM0 级事件处理

通过 Javascript 指定事件处理程序的传统方式，所有浏览器均支持。每个元素（包括 window，document）都有自己的事件处理程序属性，但是必须在 DOM 节点加载完之后才会有效

```
var div = document.getElementById("div");
    div.onclick = function(event) {
        console.log("div");
    };
```

#### DOM2 级事件处理程序

IE9，chrome，Firefox，Opera，Safari 均实现了 DOM2 级事件处理程序，绑定事件方法 addEventListener()接收三个参数：事件名称，事件处理函数和一个布尔值。布尔值为 true，则表示在捕获阶段调用事件处理程序；如果为 false，则表示在冒泡阶段调用事件处理程序。addEventListener 允许在同一个元素上添加多个事件处理程序。

```
 var div = document.getElementById("div");
     div.addEventListener("click", function(event) {
        alert("event bubble");
     }, false);
     div.addEventListener("click", function(event) {
        alert("event catch");
    }, true);
```

#### 类数值转化为数数组

```
function a(){
  return Array.prototype.slice.apply(arguments)
}
```

#### 不需要第三个数实现两个数据对换

```
 function swap(a,b){
   var a,b;
    b = b - a;
    a = a + b;
    b = a - b;

   return [a,b]
 }
```
ES6 方法
```
let a = 1;
let b = 2;
[a, b] = [b, a];
```

#### 数组转对象

##### 1.循环实现

```
  var arr = [1,2,4,5,6,7,8]
  function arrConvertObj(arr){
    var obj = {}
    for(let i = 0, len = arr.length; i < len ; i++){
      obj[i] = arr[i]
    }
    return obj
  }
  console.log(arrConvertObj(arr))//{0: 1, 1: 2, 2: 4, 3: 5, 4: 6, 5: 7, 6: 8}
```

```
var arr = [1,2,3,4,5,6,7,8,9]
function arrConvertObj(arr){
  var obj = {};
  arr.forEach((item,index)=>{
    obj[index] = item
  })
  return obj
}
console.log(arrConvertObj(arr))//{0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 7, 7: 8, 8: 9}
```

##### 2.ES6 Object.assigin()（浅拷贝）

```
var arr = [1,2,3,4,5,6,7,8,9]
function arrConvertObj(arr){
 return Object.assign({},arr)
}
console.log(arrConvertObj(arr))//{0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 7, 7: 8, 8: 9}
```
##### 3.二维数组转对象

```
 let solution = [['O', '0'], ['M', '1'], ['Y', '2'], ['E', '5'], ['N', '6'], ['D', '7'], ['R', '8'], ['S', '9']]
  solution.forEach(k => {
    solutionObj[k[0]] = k[1]
  });
  //{O: "0", M: "1", Y: "2", E: "5", N: "6", …}
```

##### js 模糊查询

```
function vagueSelsect(keyWord,arr){
  var retArr = []
  for(let i = 0 ,len = arr.length; i < len ; i++){
    if(arr[i].toString.indexOf(keyWord) >= 0){
      retArr.push(arr[i])
    }
  }
  return retArr;
}
```
