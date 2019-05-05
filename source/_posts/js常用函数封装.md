---
title: js常用函数封装
date: 2018-05-20 21:25:06
tags: js
---

### 手机号码验证

```js
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

```js
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

```js
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

```js
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

```js
// count 2~36
function randomWord(count){
    return Math.random().toString(count)+substring(2)
}
```

### 判断数组是否重复

```js
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

### cookie 增加 删除 获取Cookie

```js
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

```js
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

### 阻止冒泡
```js
 function stopBubble(e){
   e = e || window.event;
   if(e.stopPropagetion){//W3C
     e.stopPropagetion()
   }else{//IE
     e.cancelBubble = true;
   }
 }
```
#### 类数值转化为数数组
```
function a(){
  return Array.prototype.slice.apply(arguments)
}
```
#### 未完待续...
