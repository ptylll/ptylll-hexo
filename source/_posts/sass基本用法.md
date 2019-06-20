---
title: 'sass基本用法第一篇'
date: 2018-05-03 20:54:44
categories: 
- sass
tags: sass
---
Sass 是一个 CSS 的扩展，它在 CSS 语法的基础上，允许您使用变量 (variables), 嵌套规则 (nested rules), 混合 (mixins), 导入 (inline imports) 等功能，令 CSS 更加强大与优雅。使用 Sass 以及 Compass 样式库 有助于更好地组织管理样式文件，以及更高效地开发项目。
<!--more-->
#### 1.0 使用变量

##### 1-1. 变量声明

sass 变量声明方法 以及变量引用

```css
$blue: #0084ff;
$color:#cc6699;

body {
  background: $blue;
  padding: 20px;
  font-family: Helvetica;
  .nav{
    background: $color;
    border-radius: 4px;
    color:#fff;
    padding: 20px;
    font-size: 25px;
    text-align: center;
    transition: all 0.2s;
    margin: 0 auto;
    width: 300px;
  }
}


//编译后
body{
    background: $blue;
    padding: 20px;
    font-family: Helvetica;
}
.nav{
    background: $color;
    border-radius: 4px;
    color:#fff;
    padding: 20px;
    font-size: 25px;
    text-align: center;
    transition: all 0.2s;
    margin: 0 auto;
    width: 300px;
}
```

在线示例
<script async src="//jsfiddle.net/ptylll/9wmm8eqv/embed/html,css,result/dark/"></script>

##### 1-2. 变量名用中划线还是下划线分隔;

sass的变量名可以与css中的属性名和选择器名称相同，包括中划线和下划线。这完全取决于个人的喜好，有些人喜欢使用中划线来分隔变量中的多个词（如$highlight-color），而有些人喜欢使用下划线（如$highlight_color）。使用中划线的方式更为普遍，这也是compass和本文都用的方式。
不过，sass并不想强迫任何人一定使用中划线或下划线，所以这两种用法相互兼容。用中划线声明的变量可以使用下划线的方式引用，反之亦然。这意味着即使compass选择用中划线的命名方式，这并不影响你在使用compass的样式中用下划线的命名方式进行引用：

```css
$background-color:#ccc;
$background_color:#ccc;

//以上两种命名都是支持的

```

#### 2. 嵌套规则

##### 2-1 Sass 允许将一个 CSS 样式嵌套进另一个样式中，内层样式仅适用于外层样式的选择器范围内

```css
#main{
    color:#333;
    font-size:16px;
    .box{
        margin:20px;
        color:#fff;
    }
}

//编译为
#main{
    color:#333;
    font-size:16px;
}
#main > .box{
    margin:20px;
    color:#fff;
}
```
嵌套选择器有助于避免父选择的重复使用，嵌套使用结构清晰

##### 2-2  引用父选择器: （$）符号

```css
#main {
    $color:#f00;
    color: #333;
    with:200px;
    height: 50px;
    line-height: 50px;
    text-align: center;
    border: 1px solid #ccc; 
    &:hover { 
        border-color:$color;
        color:$color;
    }
}

//编译为
#main{
    color: #333;
    with:200px;
    height: 50px;
    line-height: 50px;
    text-align: center;
    border: 1px solid #ccc; 
}
#main:hover{
    border-color:#f00;
    color:#f00;
}
```
###### 2-2-1 (&) 必须出现在的选择器的开头位置

```
#main {
    color: black;
    &-sidebar { 
        border: 1px solid; 
    }
}

//编译为
#main{
    color:black;
}
#main-sidebar{
    border: 1px solid; 
}
```
演示
<script async src="//jsfiddle.net/ptylll/54po06wt/embed/html,css,result/dark/"></script>

##### 2-3 嵌套属性
CSS中有一些属性遵循相同的“命名空间”；比如，font-family, font-size, 和 font-weight都在font命名空间中。在CSS中，如果你想在同一个命名空间中设置一串属性，你必须每次都输出来。Sass为此提供了一个快捷方式：只需要输入一次命名空间，然后在其内部嵌套子属性。例如：

```
#main{
    font:{
        family: fantasy;
        size: 30em;
        weight: bold;
    }
}

//编译成
#main{
    font-family:fantasy;
    font-size: 30em;
    font-weight: bold;
}

```

##### 2-4 群组嵌套器

如果你需要在一个特定的容器元素内对这样一个群组选择器进行修饰。css的写法会让你在群组选择器中的每一个选择器前都重复一遍容器元素的选择器。例如：

```
.container h1, 
.container h2, 
.container h3 { 
    margin-bottom: .8em
    color:#333;
}

```
上面的例子中 container一直重复
可以用sass 群组内嵌规则简单处理

```
.container{
    h1,h2,h3{
        margin-bottom: .8em
        color:#333;
    }
}
```

##### 2-5 子组合选择器和同层组合选择器：>、+和~;

示例：
```
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
// 编译为

article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```

参考链接：
https://www.sass.hk/guide/
http://www.css88.com/doc/sass/#sassscript