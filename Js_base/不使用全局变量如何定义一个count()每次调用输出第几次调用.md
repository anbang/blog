不用全局变量，首先想到的是闭包：

``` 
function test(){
    var i=1;
    return function (){
        console.log("这是您第 "+(i++)+" 次调用count()")
    };
}
var count=test();
count();
count();
count();
count();
count();
count();
```
因为闭包内返回一个引用数据类型，外面有变量接收这个引用数据类型，不会效果；性能不怎么好

可以给函数添加自定义属性；
```
/*思路二:给函数添加自定义属性；*/
function count2 (){
    console.log ("这是您第 "+(++count2.count)+" 次调用count2()");
}
count2.count = 0;
count2();
count2();
count2();
count2();
count2();
count2();
```