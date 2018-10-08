mac下，找了一个代替windows live write的软件，但是这并不是好软件，后来切换到一个`blogo`的软件上

代码效果如下；
```javascript
/*点击按钮复制到剪贴板*/
var $page=$(“#page-demo”);
$page.on(“focus”,”.copy-to-shear-board”,function(){
//$(this).css({ “background-color”: “#5eb95e” }).select();
$(this).select();
document.execCommand(“Copy”)
});
/*
$page.on(“blur”,”.copy-to-shear-board”,function(){
$(this).css({ “background-color”: “#fff” });
});*/
```
 

在浏览器的效果如何不知道，但是在本机看的效果非常low；

下面是css的效果
```css
.wui-button-xs { height: 18px; line-height: 18px; padding-left: 3px; padding-right: 3px; font-size: 12px; border-radius: 3px; }
input.wui-button-xs { height: 20px; line-height: 18px; padding-left: 3px; padding-right: 3px; font-size: 12px; border-radius: 3px; }
button.wui-button-xs { height: 20px; line-height: 16px; padding-left: 3px; padding-right: 3px; font-size: 12px; border-radius: 3px; }

/**************** 无边框 ******************/
.wui-button-noborder { border: 0 none !important; }
```

如果有mac下好的博客离线编辑器，希望推荐下，谢谢；

下面是WLW的测试，带缩进的
```javascript
//带缩进的
window.onload = function () {
    var pres=document.getElementsByTagName('pre');
    for(var i=0;i<pres.length;i++){
        pres[i].className ="prettyprint linenums";
    }
    prettyPrint();
}

//不带缩进的
window.onload = function () {
    var pres=document.getElementsByTagName('pre');
    for(var i=0;i<pres.length;i++){
        pres[i].className ="prettyprint linenums";
    }
    prettyPrint();
}
```

```javascript
    //选择性黏贴-精简
    window.onload = function () {
        var pres=document.getElementsByTagName('pre');
        for(var i=0;i<pres.length;i++){
            pres[i].className ="prettyprint linenums";
        }
        prettyPrint();
    }
```
——
```javascript
    //选择性黏贴，保留格式
    window.onload = function () {
    var pres=document.getElementsByTagName('pre');
        for(var i=0;i<pres.length;i++){
            pres[i].className ="prettyprint linenums";
        }
        prettyPrint();
    }
```
 
```javascript
    //删除格式的
    window.onload = function () {
        var pres=document.getElementsByTagName('pre');
        for(var i=0;i<pres.length;i++){
            pres[i].className ="prettyprint linenums";
        }
        prettyPrint();
    }
```
```javascript
    //初始化
    window.onload = function () {
        var pres=document.getElementsByTagName('pre');
        for(var i=0;i< } prettyPrint(); ; pres[i].className="prettyprint linenums">

    //初始化
    window.onload = function () {
        var pres=document.getElementsByTagName('pre');
        for(var i=0;i
```
