---
title: js常见的排序算法
date: 2018-04-17 10:45:16
tags: js
categories: js
---

#### 快速排序

##### 原理

快速排序思想：先找到一个基准点（一般指数组的中部），然后数组被该基准点分为两部分，依次与该基准点数据比较，如果比它小，放左边；反之，放右边。
左右分别用一个空数组去存储比较后的数据。最后递归执行上述操作，直到数组长度<=1;
缺点：缺点是需要另外声明两个数组，浪费了内存空间资源。

```
function quickSort(arr){

    if(arr.length <= 1){
        return arr
    }
    var num = Math.floor(arr.length/2);
	var numValue = arr.splice(num,1);

    var left = [];
	var right =[];

	for(var i = 0 ;i<arr.length;i++){
        if(arr[i] < numValue){
            left.push(arr[i])
        }else{
            right.push(arr[i])
        }
	}
	return quickSort(left).concat(numValue,quickSort(right))

}
 var arr = [];
    for(var i = 0 ; i < 10000; i++){
        var num = Math.floor(Math.random()*100000);
        arr.push(num)
    }
console.time('time');
console.log(quickSort(arr));
console.timeEnd('time');
//[2, 3, 4, 6, 23, 45, 45, 56]
```

<!--more-->

#### 冒泡排序

##### 原理

冒泡排序思想：每一次对比相邻两个数据的大小，小的排在前面，如果前面的数据比后面的大就交换这两个数的位置;
要实现上述规则需要用到两层 for 循环，外层从第一个数到倒数第二个数，内层从外层的后面一个数到最后一个数。
缺点：比较次数较多，当数据达到一定条数效率比较低。

```
    function bubblSort(arr){
      var len = arr.length;

      if(len <= 1){
          return arr
      }
      var tmp;
      for(var i = 0 ;i < len;i++){
        for(var j = 0 ; j < len - 1 - i ;j++){
          if(arr[j+1] < arr[j]){
              tmp = arr[j];
              arr[j] = arr[j+1];
              arr[j+1] = tmp;
          }
        }
      }
      return arr
    }
    var arr = [];
    for(var i = 0 ; i < 10000; i++){
        var num = Math.floor(Math.random()*10000);
        arr.push(num)
    }
    console.time('time');
    console.log(bubblSort(arr));
    console.timeEnd('time');
     //[2, 3, 4, 6, 23, 45, 45, 56]

```

#### 冒泡与快排比较

当数据比较少的时候 模拟 10 调数据

```
function bubblSort(arr){
    var len = arr.length;

    if(len <= 1){
        return arr
    }
    var tmp;
    for(var i = 0 ;i < len;i++){
      for(var j = 0 ; j < len - 1 - i ;j++){
          if(arr[j+1] < arr[j]){
              tmp = arr[j];
              arr[j] = arr[j+1];
              arr[j+1] = tmp;
          }
      }
    }
    return arr
}
var arr = [];
for(var i = 0 ; i < 10000; i++){
    var num = Math.floor(Math.random()*10000);
    arr.push(num)
}
console.time('time');
console.log(bubblSort(arr));
console.timeEnd('time');

function quickSort(arr){

    if(arr.length <= 1){
        return arr
    }
    var num = Math.floor(arr.length/2);
	var numValue = arr.splice(num,1);

    var left = [];
	var right =[];

	for(var i = 0 ;i<arr.length;i++){
    if(arr[i] < numValue){
        left.push(arr[i])
    }else{
        right.push(arr[i])
    }
	}
	return quickSort(left).concat(numValue,quickSort(right))

}
  var arr = [];
  for(var i = 0 ; i < 10; i++){
      var num = Math.floor(Math.random()*100000);
      arr.push(num)
  }
console.time('time');
console.log(quickSort(arr));
console.timeEnd('time');
```

![](http://ot2pck40x.bkt.clouddn.com/%5B%7B7@BEDT6%25%602%25T%60%5DF%7BUP%7B%253.png)
1000 条数据
![](http://ot2pck40x.bkt.clouddn.com/_@WNT6%29@M62%25VSQ4C%7B7DGS8.png)
10000 条数据
![](http://ot2pck40x.bkt.clouddn.com/MJEHWA1~8XTZUAZKDPSG%28U4.png)

相比较在数据条数少的时候 冒泡排序速度略快于快速排序
当数据条数增加到一定条数 快速排序速度 优于冒泡排序

#### 选择排序

##### 算法简介：

选择排序是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

##### 实现

```
 function selectSort(arr){
  let len = arr.length;
  let temp ;
  let minIndex = 0;
  for(let i = 0; i < len - 1 ; i++){
    minIndex = i
    for(let j = i; j < len ; j++){
      if(arr[j] < arr[minIndex]){
        minIndex = j
      }
    }
    temp = arr[i]
    arr[i] = arr[minIndex]
    arr[minIndex] = temp
  }
  return arr
 }
  let arr = [9,8,7,455,254,551,23,45]
 selectSort(arr) //[7, 8, 9, 23, 45, 254, 455, 551]
```


参考链接：
https://www.2cto.com/kf/201609/548586.html