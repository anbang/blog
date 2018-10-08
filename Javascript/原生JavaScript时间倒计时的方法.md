这个思路是来源用%的方法来做的；

以前用%做过转秒的;

现在用来做倒计时方法；

需要用到的方法是getTime：获取距离1970年1月1日午夜00：00之间的毫秒差；
``` 
var targetTime=new Date("2016/01/25 16:59:59");
```
这个是优秀的写法；下面是有问题的写法；因为IE678下不兼容的；需要把-改成/才好；
```
var targetTime=new Date("2016-01-25 16:59:59");
```

```javascript
var oDiv=document.getElementById("div1");
var targetTime=new Date("2016/01/25 16:59:59");
 
var str=getTime(targetTime);
    oDiv.innerHTML="倒计时："+str;//进入后马上显示
 
var timer=setInterval(function(){
    var str=getTime(targetTime);
    oDiv.innerHTML="倒计时："+str;
},1000);
 
function getTime(targetTime){
    var nowTime=new Date();
    var diffTime=targetTime.getTime()-nowTime.getTime();
    var hour=parseInt(diffTime/(60*60*1000)),
    min=parseInt(diffTime%(60*60*1000)/(60*1000)),
    second=parseInt(diffTime/1000)%60;
    return zero(hour)+"时"+zero(min)+"分"+zero(second)+"秒";
}
function zero(val){
    return val<10?"0"+val:val
}
```