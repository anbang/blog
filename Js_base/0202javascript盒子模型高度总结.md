clientHeight: height+padding;//如果有滚动条，不加滚动条的高度；

clientWidth: width+padding;//如果有滚动条，不加滚动条的宽度；

clientLeft: 左边框的宽度（相当于border-left）//如果没有边框，就是0；

clientTop:上边框的宽度(相当于border-top)；//如果没有边框，就是0；


offsetHeight: height+padding+border

offsetWidth: width+padding+border

offsetLeft: width左侧距离offsetParent左侧的距离；（元素boder外侧，到其offsetparent()的border内侧的偏移量。）

offsetTop: Height顶部距离offsetParent顶部的距离；（元素boder外侧，到其offsetparent()的border内侧的偏移量。）

 

offsetParent:祖先，是决定元素offset属性的决定性目标；

 

scrollHeight :内容的总高度，当容器没有溢出，和clientHeight相等；溢出的时候是内容+上padding；

scrollWidth :

scrollLeft : 水平滚动的距离；

scrollTop : 元素垂直滚动距离；

 

Jquery中的获取方法：

$(“#div1”).width(); //width

$(“#div1”).innerWidth();//clientWidth

$(“#div1”).outerWidth();//offsetWidth

$(“#div1”).outerWidth(true);//offsetWidth+margin

margin在chrome浏览器中，默认值是8；

``` 
getComputerStyle(ele,null)
```
得到计算后的样式，所有样式属性和属性的值；

 

下面是获取元素CSS某个属性的方法
```
window.getComputerStyle(ele,null).height;

window.getComputerStyle(ele,null).width;
```
但是这方法在IE下是不兼容的；要写成：ele.currentStyle[attr]

下面是测试网页的，仅仅是test
```
window.getComputerStyle(ele,null).height;
window.getComputerStyle(ele,null).width;
```

一个变量没有定义和声明，不能用的；否则会报错异常；但是有一个特殊的，救赎用typeof测返回值的时候，返回的是undefined；一个变量没有声明过，是不可以直接读取或者判断的，但是如果这个变量，以属性的方式出现，是没有问题的，不报错（全局变量，都相当图window这个对象的属性，繁殖window的属性都可以看成全局变量【这里牵扯到了关键字字和保留字，先挖坑，以后可以研究下这方面】）

 

offsetLeft和offsetTop受哪些影响

margin

标准流的盒子本山也会有

定位

浮动

 

偏移量参照物，offsetParent；这个受哪些影响；

当这个元素发生lcoyout的时候；（网页布局重新生成的时候）和z-index什么时候起作用类似；