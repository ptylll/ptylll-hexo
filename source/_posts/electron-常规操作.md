---
title: electron进程通讯
date: 2019-06-19 15:35:53
tags: Electron
---
Electron分为主线程（mian）和渲染线程（Renderer）,当我们网页上需要操作执行electron系统上的命令时就需要调用主线程。当执系统上程序执行完会反馈到页面中。我们这把这一过程称之为进程通讯。解决这类需求通常需要`IPC`通讯。

#### 定义:

* 运行package.json的main脚本的进程称之为Main Process;
* 每一个网页都运行在独立的进程里，称之为Renderer Process。

#### ipcMain(主进程)

##### ipcMain模块是EventEmitter类的一个实例。 当在主进程中使用时，它处理从渲染器进程（网页）发送出来的异步和同步信息。 从渲染器进程发送的消息将被发送到该模块。

发送消息时，事件名称为channel。
回复同步信息时，需要设置event.returnValue。


<!--more-->
##### 用法
发送消息 send 通过 ```js
$win.webContents.send('')
```
接收消息
```js
//渲染进程 index.html
const {ipcRenderer} = require('electron')
ipcRenderer.send('asynchronous-message', 'ping')
ipcRenderer.on('asynchronous-reply', (event, arg) => {
  console.log(arg) // 1111
})
```

```js
//主进程main.js
  ipcMain.on('asynchronous-message', (event, arg) => {
    console.log(`${arg}cccc`)//pingcccc
    event.sender.send('asynchronous-reply', 1111)
  })
```
##### 主进程发送到渲染进程
同样也可以从主进程向渲染进程发送消息，使用的是 webContents.send方法

```js
ipcMain.on('asynchronous-message', (event, arg) => {
    console.log(arg)
    event.sender.send('asynchronous-reply',111)
  })

```