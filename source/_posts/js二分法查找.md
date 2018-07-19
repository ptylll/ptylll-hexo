---
title: js二分法查找
date: 2018-07-19 10:44:29
tags: js算法
---

二分法查找的前提是数据是先排列好的
实现原理：每次找到数组中的中间值做比较

已知数组 array[k]是先排列好的，array[k]中间值为 m
查找值为 T，如果 T 大于 array[m]去右边查找，如果 T 小于 array[m]去左边查找，依次递归查找。
时间复杂程度为 O(log2n)
最坏情况是 查找元素为第一个或者最后一个
最好情况为 查找元素为中间元素（奇数长度数列的正中间，偶数长度数列的中间靠左的元素）

<!--more-->

js 实现 二分法查找
循环实现
```
function find(arr, val) {
  let e = arr.length - 1;
  let s = 0;
  let m = Math.floor((s + e) / 2);
  let sortTag = arr[s] <= arr[e]
  while (s < e && arr[m] !== val) {
    if (arr[m] > val) {
      sortTag && (e = m - 1);
      !sortTag && (s = m + 1);
    } else {

    }
  }
  if (arr[m] == val) {
    console.log('找到了,位置%s', m);
    return m;
  } else {
    console.log('没找到');
    return -1;
  }
}
find([1, 2, 3, 4, 5, 6], 5)
```

递归实现

```
function find(arr,val,leftIndex,rightIndex){
  let midIndex = Math.floor((leftIndex +　rightIndex) / 2);
  let midVal = arr[midIndex];

  if(leftIndex > rightIndex){
    console.log('数据无效')
    return ;
  }else{
    if(val < midVal){
      find(arr,val,leftIndex,midVal - 1);
    }else if(val > midVal){
      find(arr,val,midVal+1,rightIndex)
    }else{
      console.log('找到位置为'+midVal)
      return
    }
  }
}
```
