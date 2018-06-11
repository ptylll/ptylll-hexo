---
title: vue各组件之间的通信
date: 2018-06-11 11:22:28
tags:
---
vue组件之间的通信方式有：父 -> 子 ，子 -> 父 ， 非父子之间的通信。
<!--more-->
##### 父子之间通信

vue父组件像子组件传递数据是用props
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
##### 子父之间通信
