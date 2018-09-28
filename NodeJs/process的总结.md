process是进程对象的；

有以下的属性；

```
*   argv 命令行参数
*   env.path 获取环境变量的值；(环境变量是在win系统的高级系统设置里面设置的；)
*
* chdir//修改当前目录
* cwd//当前目录,和dirname不一样，cwd是可以修改的；
*
*   pid: 5308,
*  kill: [Function],
*
*  stdout: [Getter],标准输出
*  stderr: [Getter],错误输出,颜色是红色的
*  stdin: [Getter],标准输入,接收用户输入
```

```
* memoryUsage:监控内存的时候用的；
* exit ;退出程序
*  nextTick: [Function: nextTick],在事件循环的下一个循环中调用callback函数；比setTimeout效率高，是在所有同步方法执行完成之后执行此回调；
*  nextTick队列会在完全执行完才调用IO操作；因此提柜的nextTick就像一个while（true）的死循环，阻止任何的IO；
*
*  setImmediate: [Function],setImmediate是在下一个周期调用，但是nexttick级别高；
*  clearImmediate: [Function],
*  同步代码>nextTick>setImmediate>定时器之类的异步代码
*
*  console: [Getter],
```
```
process.on('SIGTERM',function(){
    process.exit();
})
```

执行顺序：

### 同步代码 > nextTick > setImmediate > 定时器之类的异步代码

阻塞/非阻塞  和 异步/同步

* 非阻塞(厨师 IO) 异步(主线程 服务员)
* 阻塞(厨师 IO) 同步(主线程)
