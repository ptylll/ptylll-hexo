---
title: js两张图片合成一张
date: 2018-06-06 22:14:13
tags:
---
js利用canvas元素合成图片
主要使用到canvas上的drawImage()方法。
drawImage() 方法在画布上绘制图像、画布或视频。
drawImage() 方法也能够绘制图像的某些部分，以及/或者增加或减少图像的尺寸。

### drawImage()用法

1.在画布上定位图像：
```
drawImage(img,x,y)
```
2.在画布上定位图像设置宽高：
```
drawImage(img,x,y,with,height)
```
3.剪切图像
```
context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height);
```
<!--more-->

相关参数以及描述

| 参数 | 描述  |
| :------------: |:---------------:|
| img | 要使用的图像、画布或视频  |
| x      | 开始剪切的 x 坐标位置。|
| y | 开始剪切的 y 坐标位置。|
| sx | 开始剪切的 x 坐标位置|
| sy | 开始剪切的 y 坐标位置|
| with | 图片宽度  |
| height |图片高度。|
| swidth | 被剪切图像的宽度 |
| sheigth | 被剪切图像的高度 |

### 图片合成代码

主要js代码
```
var myCanvas = document.getElementById('myCanvas');
  var c = myCanvas.getContext('2d');

  var img = document.getElementById('img');
  img.crossOrigin = 'Anonymous';
  c.drawImage(img, 0, 0, 200, 200);
  c.font = '60px Courier New';
  c.fillText('成功了', 0, 260);
  var img02 = new Image();
  img02.src = 'http://ot2pck40x.bkt.clouddn.com/timg.jpg';
  img02.crossOrigin = 'Anonymous';
  img02.onload = function() {
    c.drawImage(img02, 200, 0, 200, 200)
  }

```
在线运行
<script async src="//jsfiddle.net/ptylll/xc0t7j15/embed/"></script>

### 总结
基本实现两张或多张图片合成但是后续合成图片路径还未做处理。等待下次总结完善

参考链接：http://www.w3school.com.cn/html5/canvas_drawimage.asp