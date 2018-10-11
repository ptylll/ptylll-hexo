---
title: js 观察者模式
date: 2018-04-16 21:46:28
tags: jsMode
categories: js
---
 
 观察者模式又叫发布---订阅模式，它定义了对象间的一种一对多的关系，让多个观察者对象同时监听某一个主题对象，当一个对象发生改变时，所有依赖于它的对象都将得到通知更新。

<!--more-->
 观察者模式优点：
    1.支持简单的广播通信，自动通知所有已经订阅过的对象
    2.页面载入后目标对象很容易与观察者存在一种动态关联，增加了灵活性。
    3.目标对象与观察者之间的抽象耦合关系能够单独扩展以及重用

 观察者模式缺点：
    1.创建订阅者需要消耗一定内存。
    2.过度使用，反而使代码不好理解及代码不好维护。

#### 实现方式

```
    var pubsub ={};
    (function(myObject){
        var topics= {},
            subUid = -1;
        //发布
        myObject.publish = function(topic,args){
            if(!topics[topic]){
                return false;
            }
            var subscribers = topics[topic],
                len = subscribers ? subscribers.length : 0;

            while(len--){
                subscribers[len].func(topic,args);
            }    
            return this;
        } 
        //订阅
        myObject.subscribe = function(topic,func){
    
            if (!topics[topic]) {
                topics[topic] = [];
            }
            var token = ( ++subUid ).toString();
            topics[topic].push({
                token: token,
                func: func
            });
            return token;
        };
        //退订
        myObject.unsubscribe = function( token ) {
            for ( var m in topics ) {
                if ( topics[m] ) {
                    for ( var i = 0, j = topics[m].length; i < j; i++ ) {
                        if ( topics[m][i].token === token ) {
                            topics[m].splice( i, 1 );
                            return token;
                        }
                    }
                }
            }
            return this;
        };
    }(pubsub))

```
实现方式
```
var messageLogger = function ( topics, data ) {
    console.log( "Logging: " + topics + ": " + data );
};
var subscription = pubsub.subscribe( "inbox/newMessage", messageLogger );
    pubsub.publish( "inbox/newMessage", "hello world!" );
    pubsub.publish( "inbox/newMessage", ["test", "a", "b", "c"] );
```

上面demo大致可以这么理解:topics为存放函数的容器，myObject.subscribe（订阅）把事件添加到topics容器中， myObject.publish（发布）是执行topics容器里面对应的事件，myObject.unsubscribe（退订）移除topics容器里的事件。
![](http://ot2pck40x.bkt.clouddn.com/~IFB%7DWP~A%5D%25Z%28TMK4837G4R.png)

利用原型特性实现方式：
```
function Pubsub(){
    this.fns=[]
}

Pubsub.prototype={
    //订阅
    subscribe:function(fn){
        this.fns.push(fn)
    },
    //发布
    publish:function(o,thisObj){
        var scope = thisObj || window;
        this.fns.forEach(function(el){
            el.call(scope, o);
        })
    },
    //退订
    unsubcribe:function(fn){
        this.fns = this.fns.filter(
            function (el) {
                if (el !== fn) {
                    return el;
                }
            }
        );
    }
}
 
```
测试
```

var o = new Pubsub();

var f1 = function(data){
	console.log('第一条订阅：'+ data )
};
var f2 = function(data){
	console.log('第二条订阅：'+ data)
};

o.subscribe(f1);
o.subscribe(f2);

o.publish("订阅成功");
o.unsubcribe(f1);
o.publish("订阅成功");
```
![运行结果](http://ot2pck40x.bkt.clouddn.com/NV%29%60%7BNZPJ%5B8%7D$VB4_O1CRHT.png)
参考链接:
http://www.cnblogs.com/TomXu/archive/2012/03/02/2355128.html
https://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript