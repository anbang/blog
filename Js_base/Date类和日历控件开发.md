```javascript
new Date()//获取客户端当前的事件（标准的事件格式数据）
getFullYear();//获取四位年
getMonth();//获取月0-11代表1-12月；
getDate();//获取日
getDay();//获取星期0-6代表周日-周六
getHours();//获取小时
getMinutes();//获取分
getSeconds();//获取秒
getMilliseconds();//获取毫秒
getTime();//获取距离1970年一月一日午夜的毫秒差；
```
 


日历开发

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>日历控件</title>
    <style>
        body{-webkit-user-select: none;}
#calendar{width: 200px;padding: 5px;background: orange;position: absolute;z-index: 100;}
#calendar h6{font-size: 16px;background: blue;color:#FFF;height: 30px;line-height: 30px;text-align: center; padding: 0;margin: 0;position: relative;cursor: pointer;}
#calendar h6 span{width: 35px;height: 30px;position: absolute;top:0;}
#calendar h6 span.prev{left: 0;background: #000000;}
#calendar h6 span.next{right: 0;background: #000000;}
#calendar ul{padding: 0;margin: 0;list-style: none;overflow: hidden;}
#calendar ul li{float: left;width: 26px;height: 26px;background: darkgreen;line-height: 25px;text-align: center;border: 1px solid #CCC;cursor: pointer;color: #FFF;}
</style>
</head>
<body>
<input type="text" value=" " onfocus="creatCalendar(this)" >
<div style="width: 200px;height: 200px; background: #CCC;"><a href="#">查看本页实现原理</a></div>
</body>
</html>
<script>
    function creatCalendar(ele){
var obj=offset(ele);//为了确定日历控件出现的位置；
        var x=obj.left;
var y=obj.top+ele.offsetHeight+5;
if(!document.getElementById("calendar")){
var calendar=document.createElement("div");
calendar.id="calendar";
calendar.style.left=x+"px";//确定日历控件出现的位置,需要用到绝对定位，并且z-index要设施；
            calendar.style.top=y+"px";
var h6=document.createElement("h6");
var prev=document.createElement("span");
var title=document.createElement("div");
var next=document.createElement("span");
prev.className="prev";
next.className="next";
prev.innerHTML="上";
next.innerHTML="下";
 
calendar.appendChild(h6);
h6.appendChild(prev);
h6.appendChild(title);
h6.appendChild(next);
document.body.appendChild(calendar);
/*            ele.onblur= function () {//如果需要鼠标离开日历界面，或者失去焦点再关闭，用到事件委托；思路参考；http://taobao.fm/works/xiaomiNavigation/index.html
                document.body.removeChild(calendar);
                calendar=null;
            };*/
            oUl=document.createElement("ul");
var currentDate=new Date;//当前的日.不让它变；
            var currentYear=currentDate.getFullYear();
var currentMonth=currentDate.getMonth();
prev.onclick=function(){active(--currentMonth)};
next.onclick=function(){active(++currentMonth)};
active(currentMonth);
calendar.appendChild(oUl);
        }
function active(m){
oUl.innerHTML="";
var activeDate=new Date(currentYear,m);//活动的日期,这个是不断变的；
            activeDate.setDate(1);//活动显示日期的日设置为1号；
            var diff=1-activeDate.getDay();//获取1号前面还有几个LI用来漏出上一个月的日期；
            var month=activeDate.getMonth();
title.innerHTML=activeDate.getFullYear()+"年"+(month+1)+"月";
activeDate.setDate(diff);//算出日历的起始日期；确定后再循环出后面的日期就可以了；算出这个月要往前退几步；
            for(var i=0;i<42;i++){
var oLi=document.createElement("li"),
date=activeDate.getDate();
oLi.innerHTML=date;//表示当前的这个LI是几号；
                oLi.dateValue=activeDate.getFullYear()+"-"+(activeDate.getMonth()+1)+"-"+date;
oLi.onclick= function () {
                    ele.value=this.dateValue;
document.body.removeChild(calendar);
calendar=null;
                };
oUl.appendChild(oLi);
if(activeDate.getMonth()!=month){
oLi.style.color="#CCC"
                }
activeDate.setDate(date+1);//让日期对象往后走一天；然后再下一次循环的时候，赋值给oLi.innerHTML
            }
        }
function offset(ele){
var l=ele.offsetLeft;
var t=ele.offsetTop;
var p=ele.offsetParent;
while(p){
if(window.navigator.userAgent.indexOf("MISE 8")>-1){
l+= p.offsetLeft;
t+= p.offsetTop;
                }else{
l+= p.offsetLeft+ p.clientLeft;
t+= p.offsetTop+ p.clientTop;
                }
p= p.offsetParent;
            }
return {left:l,top:t}
        }
    }
</script>
```