nodejs是做什么的？

# node.js是什么
- node.js 是一个让JS运行浏览器之外的服务端平台
- node第一个作用是解析执行JS代码
- node还实现了诸如文件系统、模块、包、操作系统API，HTTP服务器这些功能
- node第二个作用是一个工具库
 


# 回调函数

异步编程的基本方法，采用后续函数传递的方式，把后续的处理函数作为参数传到当前的方法内。

# 同步和异步

是对于主线程来说的

同步：任务的执行顺序与任务的排列顺序是一致的。

* readFile读取文件
* err 错误对象
* data 读取到的文件内容

```javascript
var fs = require('fs');// file system
console.log('a');
var count = 0;
fs.readFile('./fish','utf8',function(err,data){
  console.log(1,'fish');
    if(++count==2){
        eat();
    }
});
fs.readFile('./salt','utf8',function(err,data){
    console.log(2,'salt');
    if(++count==2){
        eat();
    }
});
function eat(){
    console.log('eat fish + salt');
}
console.log('b');
console.log('c');
```
输出的结果是：
- a
- b
- c
- 1 ‘fish’
- 2 ‘salt’
- eat fish + salt

### IO

- input 输入 写入一个文件
- output 输出 从一个文件里读取内容

### 单线程多线程

同一个时间内只能执行一个任务；单线程按照顺序执行，多线程可以同时执行；

### 阻塞和非阻塞

当前任务是否阻塞后面代码的执行；

非堵塞向内核发起请求的时候不会阻塞主线程的执行；比如服务员向出事下单的时候，不会阻塞服务员的行动；

### 事件

node里会触发很多的事件

### 基于事件的回调

$(‘#btn’).click(function(){});



















