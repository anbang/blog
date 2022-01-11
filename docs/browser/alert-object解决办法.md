如果直接输入alert(xxxx);如果xxx是一个对象的话，

这时候弹出来的内容是

[object Object]

此时需要转一下；

有2种解决办法：

1、如果是确定json格式的，那么可以使用

``` 
alert(JSON.stringify(jsonTep));

```

2、如果不确定是JSON格式，只能确定是一个对象，我是使用for循环出来的；比如我想输出pageUtility 这个对象看看；

代码如下

```javascript
var tarStr="";
for (i in pageUtility){
    tarStr+=i+":"+pageUtility[i].toString()+"\n";
}
alert(tarStr);
```

第二种方法是无论是不是JSON格式都可以输出的

