需求描述：点击A页面的a标签，跳转到B页面，并展现B页面中指定的选项卡；

1）、最基础的控制是，A页面的a标签，连接里面，加上#idName,这样就可以直接展现到B页面的ID位置；

这种通过锚点来实现的方法，只能应用于B页面中block情况的元素；

如果是控制B页面中选项卡的指定选项展现；

2）、可以通过url传参来实现；
```
index.html?name=value

```

这样的写法来实现，a标签的href中连接后面加上?参数；

然后再B页面的js里面判断?后面的数值来做；

```javascript
var locationName=window.location.href.toString();
var tabNameNum=locationName.split("=")[1];//如果传过来的是?name=0;那么tabName的值是0
if(tabNameNum!=null){
    (function ss(){
        tabChange(tabNameNum);
    })();
}
```

上面的locationName是获取url地址，并且转为字符串模式；

tabNameNum获取等号后面的value值；

根据tabNameNum的值来判断不同的展现；