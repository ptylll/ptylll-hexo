---
title: css userSelect 之页面禁止复制粘贴
date: 2018-09-12 11:59:11
tags: css
categories: css
---

user-select 为 css3 新增属性，主要是用于设置用户对文本选中控制。

#### 语法

```
user-select:none|text|all|element
none:表示不可选中。
text:（默认）表示文本可选中。
all:当所有内容作为一个整体时可以被选择。如果双击或者在上下文上点击子元素，那么被选择的部分将是以该子元素向上回溯的最高祖先元素。
element:可以选择文本，但选择范围受元素边界的约束。
```
<!--more-->
#### 兼容性

. IE6~9 不支持该属性使用 onselectstart='return false'来达到 user-select:none 的效果
. Opera12.5 仍然不支持该属性，但和 IE6-9 一样，也支持使用私有的标签属性 unselectable="on" 来达到 user-select:none 的效果；unselectable 的另一个值是 off；
兼容性写法
. 除 Chrome</font>和 Safari</font>外，在其它浏览器中，如果将文本设置为 -ms-user-select:none;，则用户将无法在该文本块中开始选择文本。不过，如果用户在页面的其他区域开始选择文本，则用户仍然可以继续选择将文本设置为 -ms-user-select:none; 的区域文本；
兼容写法：

```
.user{
  -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
<div class="user" onselectstart='return false' unselectable="on"></div>
```
#### 在线代码
<script async src="//jsfiddle.net/ptylll/o786cru0/embed/html,css,result/"></script>
参考链接：
http://www.css88.com/book/css/properties/user-interface/user-select.htm
