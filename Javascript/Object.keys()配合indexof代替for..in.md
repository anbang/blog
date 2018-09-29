判断某个值是不是一个object的key；

可以使用for…in的写法；

也可以使用Object.keys()配合indexof来实现，而且这个方法更效率，更优雅；

代码如下

```js
var obj={
    "admin":"1234566",
    "user1":"666666",
    "user2":"888888"
};
var  inputVal="user22";
if(Object.keys(obj).indexOf(inputVal) > -1){
    console.info(inputVal+"存在于obj;")
}else {
    console.info(inputVal+"不在obj中;")
}
```

API参考：

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys

https://msdn.microsoft.com/library/ff688127(v=vs.94).aspx