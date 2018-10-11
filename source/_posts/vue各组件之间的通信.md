---
title: vue各组件之间的通信
date: 2018-06-11 11:22:28
tags: vue 
categories: vue
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
在线实例：

<script async src="//jsfiddle.net/ptylll/09phuamd/embed/"></script>

##### 子父之间通信

我们要通过子组件改变数据，vue 中是单向性传递数据的，如果子组件想改变父组件的数据，只能通过触发事件来通知父组件改变数据。
实现方法上利用$on 和$emit。

```
//子组件
Vue.component('cnodes',{
	
	template:`
  	<div>
    	<h3>子组件</h3>
      <button @click="ChildsExample">点击传值给父组件</button>  
    </div>
    `,
    data:function(){
    	return{
      	counter:0
      }
    },
    methods:{
    	ChildsExample:function(){
        this.$emit('parantsexample','向父组件传递数据')
      }
    }
})

//父组件
new Vue({
  el: "#app",
  data:function(){
   return{
    msg:'hello Vuejs',
    total:0
   }
  },
  methods: {
  	changes:function(msg){
    	alert('从子组件穿过来的值'+'======'+msg)
    }
  }
})

```
在线实例:
<script async src="//jsfiddle.net/ptylll/2myz3j8k/embed/"></script>


##### 兄弟之间的通信
vue兄弟之jian通信除了vuex外，可以使用event Bus实现
实现基本原理是，使用一个空的Vue实例作为中央事件总线，然后在两个组件之间引入这个中央事件总线，emit,on相对应事件。
```
var bus = new Vue();

bus.$emit('selected',1)
bus.$on('selected',function(id){

})
```
实例
```
var bus = new Vue();

Vue.component('example01',{
	methods:{
  	sendMsg:function(){
	    bus.$emit('sendMsg02','发送数据到Example02')
    }
  },
  template:`
  	<div>
  		<h3>我是example01</h3>
      <button @click="sendMsg">点击我发送数据到example02</button>
    </div>`
})

Vue.component('example02',{
	template:`
  	 <div>
    	<h1>我是Example02</h1>
     </div>
  `,
  mounted:function(){
  	bus.$on('sendMsg02',function(msg){
    	alert('我是example02收到传过来的数据'+"========"+msg)
    })
  }
})

new Vue({
  el:"#container",
  data:{
    msg:"Hello VueJs"
  }
})
```
在线实例：
<script async src="//jsfiddle.net/ptylll/znkrd4vx/embed/"></script>