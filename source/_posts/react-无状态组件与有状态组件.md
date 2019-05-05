---
title: react 无状态组件与有状态组件
date: 2018-12-21 10:59:49
tags: react
---
#### 无状态组件

#### 概念 
  无状态组件是通过无状态的函数创建，它只是一种负责展示的纯组件，一般用于按钮、标签、输入框等纯展示组件，无状态组件本事不需要state来管理状态，数据是通过props传递进来，再通过render渲染。

#### 特点

  * 组件本身并无状态（state）
  * 相同的props输入会得到相同的输出结果
  * 组件本身并没有生命周期
  * 一般用于按钮，输入框等复用性较强的组件
<!--more-->
##### 语法：

ES5 写法
```js
class FastReplyList extends React.Component {
  render() {
    const {id,context,sendMessage} = this.props
    return (
      <div
        className='chichat-com-list'
        key={id}
        onClick={e => sendMessage(e)}
      >
        {context}
      </div>
    )
  }
}
```

ES6写法：

```js
const FastReplyList = props => (
  <div
    className='chichat-com-list'
    key={props.id}
    onClick={e => props.sendMessage(e)}
  >
    {props.context}
  </div>
)
```

#### 有状态组件 

##### 概念

  有状态组件是由无状态组件的内部添加了状态（state）、生命周期以及逻辑处理等构建而成。

##### 特点

* 组件内部有自身的状态（state）
* 组件内部包含生命周期
* 相同的props输入可能得到不同的结果输出
* 一般用于业务逻辑与交互数据

##### 语法：

ES5写法

```js
  let FasetReplyList = React.createClass({
    getDefaultProps:function(){
      return{
        id:1,
        context:'',
      }
    }
    render(){
      return(
        <div
            className='chichat-com-list'
            key={props.id}
            onClick={e => props.sendMessage(e)}
          >
            {props.context}
        </div>
      )
    }
  })
```

ES6写法

```js
class FastReplyList extends React.Component{
  getDefaultProps= {
    id:1,
    context:'',
  }
  render(){
    const {id,SendMessage,context} = this.props
    return(
      <div
          className='chichat-com-list'
          key={id}
          onClick={e => sendMessage(e)}
      >
        {context}
      </div>
    )
  }
}
```

#### propType 与 defaultProps 在组件中的用法

propType用于检测组件传入props的类型,当检测到传入的值无效时浏览器会抛出错误，一般用于开发模式。
propType 基本验证器：

```js
import propTypes from 'prop-types'

class FastReplyList extebds React.Component{
  propTypes:{
    optionalArray: PropTypes.array,
    optionalBool: PropTypes.bool,
    optionalFunc: PropTypes.func,
    optionalNumber: PropTypes.number,
    optionalObject: PropTypes.object,
    optionalString: PropTypes.string,
    optionalSymbol: PropTypes.symbol,

    // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
    optionalNode: PropTypes.node,

    // 一个 React 元素
    optionalElement: PropTypes.element,

    // 你也可以声明属性为某个类的实例，这里使用 JS 的
    // instanceof 操作符实现。
    optionalMessage: PropTypes.instanceOf(Message),

    // 你也可以限制你的属性值是某个特定值之一
    optionalEnum: PropTypes.oneOf(['News', 'Photos']),

    // 限制它为列举类型之一的对象
    optionalUnion: PropTypes.oneOfType([
      PropTypes.string,
      PropTypes.number,
      PropTypes.instanceOf(Message)
    ]),

    // 一个指定元素类型的数组
    optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

    // 一个指定类型的对象
    optionalObjectOf: PropTypes.objectOf(PropTypes.number),

    // 一个指定属性及其类型的对象
    optionalObjectWithShape: PropTypes.shape({
      color: PropTypes.string,
      fontSize: PropTypes.number
    }),

    // 你也可以在任何 PropTypes 属性后面加上 `isRequired` 
    // 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
    requiredFunc: PropTypes.func.isRequired,

    // 任意类型的数据
    requiredAny: PropTypes.any.isRequired,

    // 你也可以指定一个自定义验证器。它应该在验证失败时返回
    // 一个 Error 对象而不是 `console.warn` 或抛出异常。
    // 不过在 `oneOfType` 中它不起作用。
    customProp: function(props, propName, componentName) {
      if (!/matchme/.test(props[propName])) {
        return new Error(
          'Invalid prop `' + propName + '` supplied to' +
          ' `' + componentName + '`. Validation failed.'
        );
      }
    },

    // 不过你可以提供一个自定义的 `arrayOf` 或 `objectOf` 
    // 验证器，它应该在验证失败时返回一个 Error 对象。 它被用
    // 于验证数组或对象的每个值。验证器前两个参数的第一个是数组
    // 或对象本身，第二个是它们对应的键。
    customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
      if (!/matchme/.test(propValue[key])) {
        return new Error(
          'Invalid prop `' + propFullName + '` supplied to' +
          ' `' + componentName + '`. Validation failed.'
        );
      }
    })
  }
}
```

无状态组件使用方法

```js
const  FastReplyList = props =>{
  /.../
}
FastReplyList.propTypes={
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,
  /..../
}
```

#### 属性默认值

为组件添加默认属性

```js
  FastReplyList.defaultProps={
    name:'张三'
  }
```
