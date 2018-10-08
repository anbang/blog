事件对象：存储当前行为本身的相关信息；

当我们出发一个事件的时候，浏览器会执行给事件绑定的那个方法，而且在执行的时候默认给这个方法传递一个参数值，这个参数就是我们的事件对象；

DOM事件的兼容性非常多；事件对象本事就有兼容性问题；
``` 
oDiv.onclick = function (e) {}
```
先检测下事件对象是什么东西；
```
console.dir(arguments);//0:MouseEvent 鼠标事件对象
console.log(e.type);//事件类型，例如："click"
```
传进来的是一个鼠标事件对象；

事件对象本身的兼容，非标准浏览器下是window下的event对象；可以用下面的代码来解决兼容性问题；

事件对象本身的兼容：
```
e = e || window.event;//事件对象本身的兼容，非标准浏览器下是window下的event对象
```
事件源：
```
var tar = e.target || e.srcElement;//事件源
```
距离当前屏幕左上角的Z轴和Y轴的距离（不管当前是第几屏，我们获取的是当前屏的左上角）
```
e.clientX
e.clientY
```
距离body左上角的X轴和Y轴的距离（也就是第一屏左上角）
```
e.pageX
e.pageY
```

但是这个方法在非标准浏览器下不存在的；可以用下面的代码来解决兼容性问题；
```
var pageX = e.pageX;
if (!pageX) {
    pageX = e.clientX + (document.documentElement.scrollLeft || document.body.scrollLeft);
};
var pageY = e.pageY;
if (!pageY) {
    pageY = e.clientY + (document.documentElement.scrollTop || document.body.scrollTop);
};
```
阻止默认行为和阻止事件冒泡传播；

阻止默认行为：
```
//阻止默认行为
e.preventDefault();
e.returnValue=false;
e.preventDefault ? e.preventDefault() : e.returnValue = false;
```
阻止事件冒泡传播；
```
e.stopPropagation();
e.cancelBubble=true;
e.stopPropagation ? e.stopPropagation() : e.cancelBubble = true;
```

档前键盘按下的键位所对应的ASCII码值：
```
e.keyCode 
```

实际的应用，可以组织link的跳转；并输出指定的内容；
```
<a id="link" href="http://www.baidu.com/">百度</a>
<div id="div1"></div>
<script type="text/javascript">
    var oDiv = document.getElementById("div1");
        document.body.onclick = function () {
            console.log("body");
        };
        oDiv.onclick = function (e) {
            e = e || window.event;
            e.stopPropagation ? e.stopPropagation() : e.cancelBubble = true;//阻止冒泡；输出的是oDiv；
            console.log("oDiv");
        };
        var link = document.getElementById("link");
        link.onclick = function (e) {
            console.log(this.innerHTML);
            e = e || window.event;
            e.preventDefault ? e.preventDefault() : e.returnValue = false;//组织默认行为；输出的是百度、body
        };
</script>
```