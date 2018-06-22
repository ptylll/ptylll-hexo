---
layout: 'css3'
title: 'css3 text-fill-color text-stroke -webkit-linear-gradient -webkit-background-clip属性'
date: 2018-06-22 15:30:22
tags:
---

### -webkit-text-stroke 文字描边

#### 语法

text-stroke：<'text-stroke-width'> || <'text-stroke-color'>

<'text-stroke-width'>：设置或检索对象中的文字的描边厚度
<'text-stroke-color'>：设置或检索对象中的文字的描边颜色

#### 实例

```
 <div class="text text02">
   Text-fill-color
  </div>

<style>
.text02 {
  margin-top: 20px;
  color:#333;
  text-stroke: 3px #f00;
  -webkit-text-stroke: 3px #f00;
}
</style>
```
<!--more-->

### -webkit-fill-color 文字填充

#### 语法

-webkit-fill-color:<color>
<color>：指定文字的填充颜色。

```
<div class="text text03">
  Text-fill-color
</div>
<style>
  .text03{
    margin-top:20px;
    text-fill-color:#f00;
    -webkit-text-fill-color:#f00;
  }
</style>
```

### -webkit-background-clip 背景裁剪

background-clip: border-box|padding-box|content-box;

border-box	背景被裁剪到边框盒。
padding-box	背景被裁剪到内边距框。
content-box	背景被裁剪到内容框。

##### background-clip:text

```
<div class="text backgroundClip">
  background-Clip
</div>
<style>
.backgroundClip{
  margin-top:40px;
  font-size:60px;
  font-weight:800;
  height:400px;
  width:600px;
  line-height:400px;
  text-align:center;
  border:1px solid #333;
  background-image:url('http://pic.kpmapp.com/146760643196467');
  background-size:cover;
  background-clip:text;
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
</style>
```

### linear-gradient

#### 语法

<linear-gradient> = linear-gradient([ [ <angle> | to <side-or-corner> ] ,]? <color-stop>[, <color-stop>]+)
<side-or-corner> = [left | right] || [top | bottom]
<color-stop> = <color> [ <length> | <percentage> ]?

<angle>：
用角度值指定渐变的方向（或角度）。
to left：
设置渐变为从右到左。相当于: 270deg
to right：
设置渐变从左到右。相当于: 90deg
to top：
设置渐变从下到上。相当于: 0deg
to bottom：
设置渐变从上到下。相当于: 180deg。这是默认值，等同于留空不写。
<color-stop> 用于指定渐变的起止颜色：
<color>：
指定颜色。
<length>：
用长度值指定起止色位置。不允许负值
<percentage>：
用百分比指定起止色位置。


在线代码
<script async src="{embedSrc}"></script>

参考链接：
http://www.css88.com/book/css/properties/only-webkit/text-fill-color.htm
http://www.css88.com/book/css/properties/only-webkit/text-stroke.htm
https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient