---
title: js常用函数封装
date: 2018-05-20 21:25:06
tags:
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

#### 未完待续...