---
title: 'sass基本用法第二篇'
date: 2018-05-04 21:30:49
tags: sass
---
上篇介绍sass的变量 嵌套规则 嵌套属性
本篇介绍sass 使用SASS部分文件  静默注释 混合器
<!--more-->
##### sass 导入文件

在css中我们引入其他的css文件会用 @import ,这样做有一个缺点，只有当执行到@import的时候，浏览器才会去执行下载别的css文件,导致页面加载速度很慢。
sass 也有 @import 规则，但是不同的是，sass在css生成文件时候就把相关文件导入进来，这样得目的是吧多个css样式归纳到同一个css 文件中,不用去额外的去请求下载。
使用sass的@import规则并不需要指明被导入文件的全名。你可以省略.sass或.scss文件后缀（见下图）。这样，在不修改样式表的前提下，你完全可以随意修改你或别人写的被导入的sass样式文件语法，在sass和scss语法之间随意切换。举例来说，@import"sidebar";这条命令将把sidebar.scss文件中所有样式添加到当前样式表中。

```
@import "colors";
@import "grid";
@import "mixin";
//上面相当于 sass引入了三个文件

colors.sass
grid.sass
mixin.sass
```
###### sass 默认变量值 （!default）

一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值。
如果分配给变量的值后面添加了!default标志 ，这意味着该变量如果已经赋值，那么它不会被重新赋值，但是，如果它尚未赋值，那么它会被赋予新的给定值。
示例:
```
$blue: #0084ff;
$blue-darker: darken($blue, 5);
$color: #f00;
$color: #ccc !default;
body {
  background: #20262E;
  padding: 20px;
  font-family: Helvetica;
}

#box{
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  font-size: 25px;
  text-align: center;
  transition: all 0.2s;
  margin: 0 auto;
  width: 300px;
  color:$color; 
 }
 上面的代码表示
 如果$color未定义的话 color默认颜色#ccc;如果另外设定了$color样式,默认的$color:#ccc会被覆盖掉
```
<script async src="//jsfiddle.net/ptylll/hnwL1xhx/embed/html,css,result/"></script>

###### sass嵌套导入
由于sass有@import 规则。这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方。

示例 theme-bule.sass内容如下
```
    aside{
        color:#ccc;
        background-color:#f00;
    }
```
另一个 mixin.sass 文件
```
 .bule-theme{
    @import "theme-bule"
 }

 //上面代码编译为

 .bule-theme{
    aside{
        color:#ccc;
        background-color:#f00;
    }
 }
```

##### 静默注释 (/* */ 和 //)
Sass 支持标准的CSS多行注释以/* */以及单行注释 //。在尽可能的情况下，多行注释会被保留在输出的CSS中，而单行注释会被删除。
```
* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body { color: black; }

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.
a { color: green; }
//编译为
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body {
  color: black; }

a {
  color: green; }
```

##### 混合器（mixin）
混合器(mixin)允许您定义可以在整个样式表中重复使用的样式，而避免了使用无语意的类（class），比如 .float-left。混入(mixin)还可以包含所有的CSS规则，以及任何其他在Sass文档中被允许使用的东西。 
他们甚至可以带arguments，引入变量，只需少量的混入(mixin)代码就能输出多样化的样式。

###### 混合器定义（@mixin）
混合器定义通过@mixin指令
定义一个简单的清除浮动混合器
```
@mixin clearfix{
    display:block;
    &:before,
    &:after{
        content:'';
    },
    &:after{
        clear:both;
    }
}
```
```
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}
```

###### 引入混合器（@include）

使用 @include 指令可以将混入（mixin）引入到文档中。这需要一个混入的名称和可选的参数传递给它，并包括由混入定义的当前规则的样式,混入（mixin）定义也可以包含其他的混入. 例如：

```
#main{
    @include large-text;
    @include clearfix;
    padding:20px;
    margin:5px;
}
```
###### 混合器传参

```
@mixin links-colors($normal,$hover,$visite){
    color:$normal
    &:hover{
        color:$hover
    }
    &:visite{
        color:$visite
    }
}
```
引入
```
 a{
    @include links-color(#ccc,#f00,#999);
 }
 //编译成

 a{color:#ccc}
 a:hover{color:#f00}
 a:visted{color:#999}
```

######  默认参数值

为了在@include混合器时不必传入所有的参数，我们可以给参数指定一个默认值。参数默认值使用$name: default-value的声明形式，默认值可以是任何有效的css属性值，甚至是其他参数的引用，如下代码：

```
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```