---
title: vue各组件之间的通信
date: 2018-06-11 11:22:28
tags:
---

vue 组件之间的通信方式有：父 -> 子 ，子 -> 父 ， 非父子之间的通信。

##### 父子之间通信

vue 父组件像子组件传递数据是用 props

```
//父组件
<template>
  <header-Mian :title-text="titleText"></header-Mian>
</template>
<script>
  import Header from './header';
  export default{
    name:'index',
    components:{
      'header-Mian':Header
    },
    data(){
      return {
        titleText:'首页'
      }
    }
  }
</script>

//子组件

<template>
    <header>
        {{thisTitleTxt}}
    </header>
</template>
<script>
    export default {
        name: 'header-Mian',
        props: {
            titleText: String
        },
        data () {
            return {
                thisTitleTxt: this.titleText
            }
        }
    }
</script>
```

<!--more-->

##### 子父之间通信

我们要通过子组件改变数据，vue 中是单向性传递数据的，如果子组件想改变父组件的数据，只能通过触发事件来通知父组件改变数据。
实现方法上利用$on 和$emit。

```
//父组件
<template>
  <div id="counter">
    <p>{{total}}</p>
    <button-counter v-on:increment="incrementTotal"></button-counter>
  </div>
</template>
<script>
 import 'ButtonCounter' from './buttoncounter';
  export default{
  name:'index',
    components: {
        'button-conuter': ButtonCounter
      },
    data () {
      return {
          total: 0
      }
    },
    methods: {
      incrementTotal () {
          this.total++
      }
    }
 }
</script>

//子组件
<template>
  <button @click="incrementCounter">{{counter}}</button>  
</template>
<script>
  export default {
    name: 'button-counter',
    data () {
        return {
            counter: 0
        }
    },
    metheds: {
        incrementCounter () {
            this.$emit('increment')
            this.counter++
        }
    }
  }
<script>
```
##### 兄弟之间的通信
vue兄弟之jian通信除了vuex外，可以使用eventBus实现