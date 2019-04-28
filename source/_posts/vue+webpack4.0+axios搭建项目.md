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
  mode: 'development'
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
  mode: 'production'
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
#### 3.拓展功能
以上配置已经完成了一个简易的本地开发环境，当我们需要写在项目中添css 图片等等，这个环节目前都是不支持的，所以我们需要在现有的环境中拓展功能。

##### 1.安装loader (css-loader, sass-loader, style-loader,postcss-loader,node-sass,autoprefixer,node-sass ,file-loader)
```
yarn add css-loader sass-loader style-loader postcss-loader autoprefixer node-sass file-loader
```
```
//当前版本
"css-loader": "^2.1.1",
"postcss-loader": "^3.0.0",
"sass-loader": "^7.1.0",
"style-loader": "^0.23.1",
"autoprefixer": "^9.5.1",
"node-sass": "^4.11.0",
```

处理css 
```
css-loader
style-loader
```
处理sass
```
sass-loader
node-sass
```
css兼容
```
"postcss-loader": "^3.0.0",
"autoprefixer": "^9.5.1",
```
并在根文件夹创建 postcss.config.js 文件
```
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}
```
webpack.base.js中添加
```
//处理sass scss css 
/**/
 rules: [{
        //sass scss css 处理
        test: /\.(sa|sc|c)ss$/,
        use: [
          'style-loader',
          'css-loader',
          'postcss-loader',
          'sass-loader',
        ],
      },
    ]
/**/    
```
解析文件图片
```
/**/
rules: [{
    test: /\.(png|svg|jpg|gif)$/,
    use: [
      {
        loader: 'file-loader',
        options: {
          limit: 5000,
          // 分离图片至imgs文件夹
          name: "imgs/[name].[ext]",
        }
      },
    ]
  }],
/**/
```

#### 4.webpack 构建打包优化

1.解决每次重新打包，dist 文件夹文件未清除

```
yarn add clean-webpack-plugin
```
当前版本
```
 "clean-webpack-plugin": "^2.0.1"
```

```
//webpack.prod.js

const CleanWebpackPlugin = require('clean-webpack-plugin');

//**
plugins:[
  //**
  new CleanWebpackPlugin(['dist/*'], {
    root: path.resolve(__dirname, '../')
  }),
  //**
]
```
2.分离 CSS

安装mini-css-extract-plugin
```
yarn add mini-css-extract-plugin
```
当前版本

```
"mini-css-extract-plugin": "^0.6.0"
```

```
//webpack.prod.js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

//**
  plugins: [
 
    new MiniCssExtractPlugin({
      filename: 'css/[name].[hash].css',
      chunkFilename: 'css/[id].[hash].css',
    }),
  ],
```
3.压缩CSS和JS代码

安装 optimize-css-assets-webpack-plugin 和 uglifyjs-webpack-plugin 插件

```
yarn add optimize-css-assets-webpack-plugin uglifyjs-webpack-plugin
```

当前版本
```
"optimize-css-assets-webpack-plugin": "^5.0.1"
"uglifyjs-webpack-plugin": "^2.1.2"
```


```
// webpack.prod.js
// 压缩CSS和JS代码
// ...省略号
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");

module.exports = merge(common, {
  // ...省略号
  optimization: {
    // ...省略号
    minimizer: [
      // 压缩JS
      new UglifyJsPlugin({
        uglifyOptions: {
          compress: {
            warnings: false, // 去除警告
            drop_debugger: true, // 去除debugger
            drop_console: true // 去除console.log
          },
        },
        cache: true, // 开启缓存
        parallel: true, // 平行压缩
        sourceMap: false // set to true if you want JS source maps
      }),
      // 压缩css
      new OptimizeCSSAssetsPlugin({})
    ]
  },
})

```
4. 配置resolve
Webpack 在启动后会从配置的入口模块出发找出所有依赖的模块，Resolve 配置 Webpack 如何寻找模块所对应的文件。 Webpack 内置 JavaScript 模块化语法解析功能，默认会采用模块化标准里约定好的规则去寻找，但你也可以根据自己的需要修改默认的规则。

```
//webpack.base.js
resolve: {
  extensions: ['.js', '.vue'],
  alias: {
    vue: 'vue/dist/vue.esm.js',
    '@': path.resolve('src')
    '$component': path.resolve('./src/component'),
    '$api': path.resolve('./src/api'),
    '$page':path.resolve('./src/page')
  }
}
```

#### 5.封装配置axios

scr 目录下添加api目录，新建http.js文件
```
import axios from 'axios'

axios.default.baseURL = ''
axios.default.timeout = 5000

//request 拦截器
axios.interceptors.request.use(config => {
    config.data = JSON.stringify(config.data);
    config.headers = {
      'Content-Type': 'application/x-www-form-urlencoded'
    }
    return config
  },
  error => {
    return Promise.reject(error)
  }
)

//response 拦截器
axios.interceptors.response.use(
  response => {
    if (response.data.errCode == 2) {
      router.push({
        path: "/home",
        querry: {
          redirect: router.currentRoute.fullPath
        } //从哪个页面跳转
      })
    }
    return response;
  },
  error => {
    return Promise.reject(error)
  }
)

export function get(url, params = {}) {
  return new Promise((resolve, reject) => {
    axios.get(url, {
        params: params
      })
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export function patch(url, params = {}) {
  return new Promise((resolve, reject) => {
    axios.patch(url, {
        params: params
      })
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export function remove(url, params = {}) {
  return new Promise((resolve, reject) => {
    axios.delete(url, {
        params: params
      })
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export function put(url, params = {}) {
  return new Promise((resolve, reject) => {
    axios.put(url, {
        params: params
      })
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export function post(url, params = {}) {
  return new Promise((resolve, reject) => {
    axios.post(url, {
        params: params
      })
      .then(res => {
        resolve(res.data)
      })
      .catch(err => {
        reject(err)
      })
  })
}
```
在index.js文件中引入方法通过Vue.protoype挂载到vue原型上
```
import { get, patch, remove, put, post } from './src/api/http.js'

Vue.prototype.$post = post
Vue.prototype.$get = get
Vue.prototype.$put = put
Vue.prototype.$delete = remove
Vue.prototype.$patch = patch
```
使用
```
//**
  mounted() {
    this.$get('/api/in_theaters').then(res=>{
      console.log(res)
    })
  },
//**
```
#### 6.总结
整体配置不算麻烦，主要是插件对应的版本以及配置方法。

参考链接：
https://juejin.im/post/5b7d350951882542f3278b11
https://webpack.github.io/