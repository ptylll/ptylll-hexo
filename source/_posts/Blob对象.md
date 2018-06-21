---
layout: js Blob 对象
title: Blob对象
date: 2018-06-19 10:49:41
tags: js
---
Blob 对象表示一个不可变、原始数据的类文件对象。Blob 表示的不一定是JavaScript原生格式的数据。File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。

#### 构造函数

var blob = new Blob(data[,options])
创建一个新的Blob对象

data必须为数组;
options是对这一Blob对象的配置属性，目前也只有一个type也就是相关的MIME需要设置 type的值： ‘text/csv,charset=UTF-8’ 设置为csv格式，并设置编码为UTF-8，’text/html’ 设置成html格式;

<!--more-->

```
var blob = new Blob([12,31,4,23,5],{type:'text/html'});
```
![](http://ot2pck40x.bkt.clouddn.com/1529416863%281%29.jpg)

#### Blob属性
```
blob.size // blob大小（字节）
blob.type // blob的MIME类型
```


参考链接：
https://developer.mozilla.org/zh-CN/docs/Web/API/Blob