---
title: koa get,post接收数据
date: 2019-05-21 15:37:29
tags: node
---

#### GET请求

在koa中，获取GET请求数据源头是koa中request对象中的query方法或querystring方法，query返回是格式化好的参数对象，querystring返回的是请求字符串，由于ctx对request的API有直接引用的方式，所以获取GET请求数据有两个途径。
<!--more-->
  1.是从上下文中直接获取
    请求对象ctx.query，返回如 { a:1, b:2 }请求字符串ctx.querystring，返回如 a=1&b=2
  2.是从上下文的request对象中获取请求对象ctx.request.query，返回如 { a:1, b:2 }请求字符串 ctx.request.querystring，返回如 a=1&b=2

  实例：
  ```js
  //server.js
const Koa = require('koa')
const app = new Koa()


app.use(async (ctx)=>{
 let url = ctx.url
 console.log(ctx.url)
  let req = ctx.request
  let req_query = req.query
 ctx.body = {
   url,
   req_query
 }
 
})

  ```
  ```js
  node server.js
  ```
  ![](https://cdn.sinaimg.cn.52ecy.cn/large/005BYqpgly1g390k343ivj30gj05kgle.jpg)

  #### POST请求

  koa对post请求并没有做封装，只能通过解析上下文context的原生Node.js请求对象req。当node.js后台收到post请求时，会以buffer的形式将数据缓存起来。Koa2中通过ctx.req.addListener('data', ...)这个方法监听这个buffer。

  实例：

```js
  //server.js
  const Koa = require('koa')
  const app = new Koa()


  app.use(async (ctx)=>{
  if(ctx.url === '/' && ctx.method === 'GET'){
      let html =`
        <form action='/' method="POST">
          <p>
            <label>名称：</label>
            <input name='user'/>
          </p>
          <p>
            <label>年纪：</label>
            <input name='age'/>
          </p>
          <p>
          <button type="submit">submit</button>
          </p>
        </form>
      `
      ctx.body = html
    }else if(ctx.url === '/' && ctx.method === "POST"){
      console.log(ctx)
      let getPost = await getPostData(ctx)

      ctx.body = getPost
    }else{
      ctx.body ='404'
    }
  
  })

  function getPostData(ctx){//处理ctx
    return new Promise((resolve,reject)=>{
      try{
        let postdata = ''

        ctx.req.addListener('data',(data)=>{
          console.log(`接收到数据${data}`)
          postdata += data
        })
        ctx.req.addListener('end',()=>{
          console.log( `接收完毕`)
          let n = queryString(postdata)
          resolve(n)
        })
      }
      catch(err){
        reject(err)
      }
    }) 
  }

  function queryString(str){//字符串转对象
    let arr = str.split('&')
    let len = arr.length 
    let obj = {}

    for(let i = 0;i<len;i++){
      let n = arr[i].split('=')
      obj[n[0]] = decodeURIComponent(n[1]) 
    }
  
    return obj
  }

  app.listen(8888,()=>{
    console.log('start...')
  })
```
  ```js
  node server.js
  ```
![](https://cdn.sinaimg.cn.52ecy.cn/large/005BYqpgly1g3914yblryj30do04hdfl.jpg)

总结
1.post请求会被node.js缓存成多个buffer对象。
2.字符串可以直接通过字符串拼接buffer对象来获取请求数据，但是文件类型的数据需要特殊的处理方式。
3.当表单中有中文的时候需要使用decodeURIComponent()方法转化

参考链接：
https://chenshenhai.github.io/koa2-note/note/request/get.html
