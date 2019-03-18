---
title: webpack 配置 resolve
date: 2019-03-18 18:05:22
tags: webpack
---

webpack在启动之后会从程序的入口出发（entry）找到依赖的文件。resolve配置webpack依赖模块如何被解析。webpack 提供合理的默认值，但是还是可能会修改一些解析的细节。

<!--more-->

#### resolve.alias 

alias 通过配置把原来导入的文件映射成一个新的路径。

```
module.exports = {
  //...
  resolve:{
    alias:{
      $api: resolve('src/api'),
      $common: resolve('src/common'),
      $components: resolve('src/components'),
      $store: resolve('src/store'),
      $reducers: resolve('src/store/reducers'),
      $actions: resolve('src/store/actions'),
      $router: resolve('src/router'),
      $style: resolve('src/style'),
      $svg: resolve('src/svg'),
      $config: resolve('src/config'),
      $sagas: resolve('src/store/sagas'),
      $compass: resolve('node_modules/compass-mixins'),
    }
  }
}
```
通过配置
```
import Demo from './src/components'
```
等价于
```
import Demo from '$cpmponents
```
例如我们有时候导入相对路径时写成：

```
import Demo from '../../../image/img'
```
通过映射直接写成
```
import Demo from 'image/img
```
以上做法虽然写法上简化了许多，但是搜索中命中率很高。alias可以通过再尾部添加$精准匹配。
```
module.exports = {
  //...
  resolve:{
    alias:{
      xyz$: path.resolve(__dirname, 'path/to/file.js')
    }
  }
}
 ```
 这将产生以下结果：
 ```
  import Test1 from 'xyz'; // 精确匹配，所以 path/to/file.js 被解析和导入
  import Test2 from 'xyz/file.js'; // 非精确匹配，触发普通解析
 
```

#### resolve.descriptionFiles

```
module.exports = {
  //...
  reslove:{
    descriptionFiles:['package.json']
  }
}
```
用于描述得JSON文件


#### resolve.mainFields 

当从 npm 包中导入模块时（例如，import * as D3 from 'd3'），此选项将决定在 package.json 中使用哪个字段导入模块。根据 webpack 配置中指定的 target 不同，默认值也会有所不同。当 target 属性设置为 webworker, web 或者没有指定默认值为：
```
module.exports = {
  //...
  resolve:{
    mainFields:['browser','module','main']
  }
}
```
对于其他任意的 target（包括 node），默认值为：
```
module.exports = {
  //...
  resolve: {
    mainFields: ['module', 'main']
  }
};
```

#### resolve.modules

告诉 webpack 解析模块时应该搜索的目录。
绝对路径和相对路径都能使用，但是要知道它们之间有一点差异。
通过查看当前目录以及祖先路径（即 ./node_modules, ../node_modules 等等），相对路径将类似于 Node 查找 'node_modules' 的方式进行查找。

使用绝对路径，将只在给定目录中搜索。

```
module.exports={
  resolve:{
    modules:['node_modules]
  }
}
```

#### resolve.extensions

在导入语句没带文件后缀时，Webpack 会自动带上后缀后去尝试访问文件是否存在。  resolve.extensions 用于配置在尝试过程中用到的后缀列表，默认是：
```
module.exports = {
  //...
  resolve: {
    extensions: ['.wasm', '.mjs', '.js', '.json']
  }
};
```

#### resolve.plugins

用于拓展额外的插件。
```
module.exports = {
  //...
  resolve: {
    plugins: [
      new DirectoryNamedWebpackPlugin()
    ]
  }
};
```

#### resolve.mainFiles

解析目录时要使用的文件名。
```
module.exports = {
  //...
  resolve: {
    mainFiles: ['index']
  }
};
```