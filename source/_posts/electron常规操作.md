---
title: electron进程通讯
date: 2019-06-19 15:35:53
categories: 
- Electron
tags: Electron
---
Electron分为主线程（mian）和渲染线程（Renderer）,当我们网页上需要操作执行electron系统上的命令时就需要调用主线程。当执系统上程序执行完会反馈到页面中。我们这把这一过程称之为进程通讯。解决这类需求通常需要`IPC`通讯。

#### 定义:

* 运行package.json的main脚本的进程称之为Main Process;
* 每一个网页都运行在独立的进程里，称之为Renderer Process。

##### ipcMain模块是EventEmitter类的一个实例。 当在主进程中使用时，它处理从渲染器进程（网页）发送出来的异步和同步信息。 从渲染器进程发送的消息将被发送到该模块。

发送消息时，事件名称为channel。
回复同步信息时，需要设置event.returnValue。


<!--more-->
#### 主进程渲染进程异步发送消息
渲染进程使用ipcRenderer.send发送异步消息，然后使用on事件监控主进程的返回值。主进程使用on事件监听消息，使用event.sender.send返回数据。
渲染进程和主进程进行关联，发送消息时，事件名称为channel。

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
输出：
主进程输出：
<div align=left>
![主进程输出](https://i.loli.net/2019/06/20/5d0af5ba1972879261.jpg)
</div>

渲染控制台输出：
<div align=left>
![渲染进程](https://i.loli.net/2019/06/20/5d0af5ba191eb59772.jpg)
</div>

#### 主进程渲染进程同步发送消息

渲染进程使用`ipcRenderer.sendSync`发送同步消息，主进程还是通过`on`事件监控，通过`event.returnValue`返回数据，渲染进程返回值直接为`ipcRenderer.sendSync`函数。

缺点：
同步方式会阻碍渲染进程渲染,尽量避免同步发送。

示例代码：
```js 
//index.js
ipcMain.on('sync-message', (event, arg) => {
  console.log(arg)
  event.returnValue = 22222
})

```
```js
//index.vue
 sync () {
    let syncMessage = ipcRenderer.sendSync('sync-message', 9999)
    console.log(syncMessage)
  }
```
渲染控制台输出：
![主进程输出](https://i.loli.net/2019/06/20/5d0af5ba1972879261.jpg)

渲染控制台输出：
![渲染进程](https://i.loli.net/2019/06/20/5d0afd52a740133902.jpg)

#### 主进程主动发送消息
在某些情况下主线程也可以主动向渲染线程发送消息。主线程通过`$winow.webContents.send`方法

示例：
```js
//index.js
//***
  function createWindow() {
  win = new BrowserWindow({
    width: 1200,
    height: 600,
    frame: false,
    webPreferences: {
      nodeIntegration: true
    }
  })
  win.loadFile('index.html')
  win.webContents.openDevTools()
  win.webContents.on('did-finish-load',()=>{
    win.webContents.send('main-message', 123123)
  })
  win.on('closed', () => {
    win = null
  })
}
```

```js
  const { ipcRenderer } = require('electron')
  ipcRenderer.on('main-message', (event, arg) => {
    console.log(arg)
  })
```
![](https://i.loli.net/2019/06/20/5d0b27b66a7dd76762.jpg)

注意：当我们通过 `win.webContents.send()`发送向渲染线程发送消息时，需要通过`win.webContents.on('did-finish-load'()=>{})`函数检测渲染线程框架导航是否完成加载。

#### 渲染线程之间发送消息

渲染线程之间通讯
##### 主线程消息中转
```js
// In the main process.
ipcMain.on('ping-event', (event, arg) => {
  yourWindow.webContents.send('pong-event', 'something');
}
// In renderer process
// 1
ipcRenderer.send('ping-event', (event, arg) => {
    // do something
  }
)
// 2
ipcRenderer.on('pong-event', (event, arg) => {
    // do something
  }
)
```
##### remote接口
利用 remote 接口直接获取渲染进程发送消息：
```js
remote.BrowserWindow.fromId(winId).webContents.send('ping', 'someThing')
```
渲染进程获取 ID 就有多种方法了：

第一种： 通过 global 设置和获取
第一种是： 主进程创建事件，发送信息
```js
// main process
win1.webContents.send('distributeIds',{
    win2Id : win2.id
});
win2.webContents.send('distributeIds',{
    win1Id : win1.id
});
```
#### WebContents
`webContents`是`EventEmitter`的实例负责渲染和控制网页,是`BrowserWindow`对象的一个属性。
#####  `webContents.getAllWebContents()`
返回`WebContents[]` - 所有 WebContents 实例的数组包含所有`Windows，webviews，opened devtools`和devtools扩展背景页的web内容

##### `webContents.getFocusedWebContents()`
`Returns WebContents`-此app中焦点的web内容否则返回null。

##### `webContents.fromId(id)`
`Returns WebContents`给定id的WebContents实例。

#### WebContents事件(常用)

##### `did-finish-load`
导航完成时触发，即选项卡的旋转器将停止旋转，并指派onload事件后

##### `did-fail-load`
导航加载失败时触发，与`did-finish-load`事件相反

##### `did-frame-finish-load`
框架导航加载完成触发

##### `did-start-loading`
当tab中的旋转指针（spinner）开始旋转时，就会触发该事件

##### `did-stop-loading`
当tab中的旋转指针（spinner）结束旋转时，就会触发该事件。

##### `did-ready`
一个框架中的文本加载完成后触发该事件。

#### 参考地址
https://github.com/electron/electron/blob/master/docs/api/web-contents.md#contentssendchannel-arg1-arg2-
https://electronjs.org/docs/api/web-contents
https://segmentfault.com/a/1190000009253100
https://electronjs.org/docs/api/remote