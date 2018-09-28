nodejs中为时间添加监听，常见的有

addlistener 和 on 两种方法；

别名的意思，一样的效果；
```javascript
const listenter1 =()=>{
    console.log("listenter1")
};
const listenter2 =()=>{
    console.log("listenter2")
}; 
eventsEmitter.addListener('connection',listenter1);
eventsEmitter.on('connection',listenter2);

``` 
如同jq里的on,bind；

个人偏向用on来处理；

随便你喜欢用哪个；

``` 
events.EventEmitter.listenerCount

``` 
这是一个计算某个事件有多少监听的计数；一半配合

``` 
addListener 
removeListener
``` 
完整的代码如下

```javascript
/**
    * Created by broszhu on 2017/6/30.
    */
const events=require('events');
const eventsEmitter=new events.EventEmitter();
    
const listenter1 =()=>{
    console.log("listenter1")
};
const listenter2 =()=>{
    console.log("listenter2")
};
    
eventsEmitter.addListener('connection',listenter1);
eventsEmitter.on('connection',listenter2);
    
let eventListeners=events.EventEmitter.listenerCount(eventsEmitter,"connection");
console.log(`连接数是${eventListeners}`);
    
eventsEmitter.emit("connection");
    
eventsEmitter.removeListener('connection',listenter1);
    
console.log("listenter1 is remove");
eventListeners=events.EventEmitter.listenerCount(eventsEmitter,"connection");
console.log(`连接数是${eventListeners}`);
    
eventsEmitter.emit("connection");
    
console.log("end");

``` 
输出的如下

``` 
连接数是2
listenter1
listenter2
listenter1 is remove
连接数是1
listenter2
end

``` 