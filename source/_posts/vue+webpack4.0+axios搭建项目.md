---
title: vue+webpack4.0+axios搭建项目
date: 2019-04-25 10:45:46
tags: vue
---

#### 1.前言

项目过程中一直使用vue-cli脚手架，本篇绕vue-cli，通过webpack4.0版本+vue2.6.10版本，加深对webpack的印象。

#### 2.项目搭建

##### 1.创建VueDemo目录，进入该目录，执行命令：
```
npm init
```
通过一系列操作目录中生成了一个package.json文件。

<!--more-->

##### 2.安装webpack

```
yarn add webpack webpack-cli webpack-dev-server webpack-merge
```

```js
//当前安装版本
"webpack": "^4.30.0",
"webpack-cli": "^3.3.1",
"webpack-dev-server": "^3.3.1",
"webpack-merge": "^4.2.1"
```
##### 3.新建目录文件

```
//项目目录
VueDemo
  |--dist //打包生成文件目录
  |--config
      |--webpack.prod.js //生产环境配置
      |--webpack.dev.js //开发环境配置
      |--webpack.base.js //webpack 公共配置
  |--src
      |--index.js //入口js文件
      |--app.vue
  |--static //静态资源文件    
  |--index.html //入口html文件
```
##### 4. 配置webpack

```
//webpack.base.js 通用配置

const webpack = require('webpack')

module.exports = {
  //入口文件
  entry: () => new Promise((resolve) => resolve(['./src/index.js'])),//这里也可以直接写成 entry:'./scr/index.js'

  //处理一些项目文件例如js css img ...
  module:{
    rules:[]
  },

  //插件
  plugins:[],

}

```

```
//webpack.dev.js
//本地开发环境配置

const path = require('path')
const merge = require('webpack-merge')
const comm = require('./webpack.base')

module.exports = merge(comm, {
  devtool: 'inline-source-map',
  server: { //本地开发node服务器
    contentBase: '../dist'
  },
  output: {
    filename: 'js/[name].[hash].js', //每次变化hash跟着改变
    path: path.resolve(__dirname, '../dist') //打包后文件夹
  },
  module: {},
  mode: 'dev'
})
```
```
//webpack.prod.js
//生产环境配置
const path = require('path')
const merge = require('webpack-merge')
const comm = require('./webpack.base')

module.exports = merge(comm, {
  plugins: [],
  module: {},
  output: {
    filename: 'js/[name].[contenthash].js', //contenthash 若文件内容无变化，则contenthash 名称不变
    path: path.resolve(__dirname, '../dist') //打包后文件夹
  },
  mode: 'prod'
})
```
##### 5.添加Vue插件

```
yarn add vue vue-loader vue-template-compiler
```
```
//当前版本
"vue": "^2.6.10",
"vue-loader": "^15.7.0",
"vue-template-compiler": "^2.6.10",
```
##### 6.配置vue文件
vue文件不管是生产环境还是开发环境都需要配置 所以放到webpack.base.js中

```
//webpack.base.js

const VueloaderPlugin = require('vue-loader/lib/plugin') // 注意vue-loader 引入有两种方式 
//另一种方式引入 const { VueloaderPlugin } = require('vue-loader')

module.exports = {
  
  /**/
  module: {
    rules: [{
      test: /\.vue$/,
      loader: 'vue-loader'
    }]
  },

  plugins: [
    new VueloaderPlugin()
  ],
  /**/
}
```
#### 7.修改index.js index.html app.vue
```
//index.js

import Vue from 'vue'
import App from './app.vue'

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})

```

```
//index.html

/**/
<body>
  <div id='app'>

  </div>
</body>
/**/

```

```
//app.vue

<template>
  <div>
    VueDemo
  </div>
</template>

```

##### 8.添加html解析插件

```
yarn add html-webpack-plugin
```

```
//当前版本
"html-webpack-plugin": "^3.2.0",
```
生产开发环境中都需要解析html,所以放到webpack.base.js中配置
```
//webpack.base.js

const HtmlWebPackPlugin = require('html-webpack-plugin')
//**
Plugins:[
  new HtmlWebPackPlugin({
    template: path.resolve(__dirname, '../index.html')
  })
]
//**
```

##### 9.配置script命令
```
//package.json文件

"scripts": {
  "start": "webpack-dev-server --hot --open --config config/webpack.dev.js",
  "build": "webpack --config config/webpack.prod.js"
},
```