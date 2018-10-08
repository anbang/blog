开发中遇到的坑
0、IE6下div图片下边4px空隙bug的解决办法
清零的时候设置img为“display:block;”这样就可以了


1、在开发商品详情页时候，个人博客鼠标移入后在IE5-7中z-index时效
原因是：IE7及更早版本对z-index的解析有个讨厌的Bug，如果父元素具有position: relative/absolute;属性，那么只设置子元素的z-index是不起作用的，父元素也得一起设置。也就是说必须把要控制元素和它上层的所有DIV都加上z-index才行。

放大镜功能，天坑合集！
1、放大镜效果中；大图片的left和top兼容问题；左右便捷判断
```javascript
//        bigImg.style.left="-"+(l/tabW*bigImg.width)+"px";
//        bigImg.style.top="-"+(t/tabH*bigImg.height)+"px";
        //如果样式用上面的写，在IE678的时候有兼容性问题，应该改为下面的
if(l<=0){
    bigImg.style.left=="0px";
}else{
    bigImg.style.left="-"+l/tabW*bigImg.width+"px";
}
if(t<=0){
    bigImg.style.top=="0px";
}else{
    bigImg.style.top="-"+(t/tabH*bigImg.height)+"px";
}
```
2、放大镜效果中，因为要兼容到IE5678，所以用一个DIV抱住元素，必须用定位；但是放大镜的镜片mark定位需要与鼠标相连；需要算出小图片的left值；此时需要用到元素距离浏览器左边框的距离；
解决办法是：算出距离左上角的绝对left和top来；然后再去操作,写一个函数解决；

```javascript
function offset() {
    var left = this.offsetLeft, top = this.offsetTop, par = this.offsetParent;
    while (par) {
        left += par.offsetLeft;
        top += par.offsetTop;
        if (window.navigator.userAgent.indexOf("MSIE 8.0") <= -1) {
            left += par.clientLeft;
            top += par.clientTop;
        }
        par = par.offsetParent;
    }
    return {
        left: left,
        top: top
    };
}
```
3、在滚动图片的时候；放大镜会错位；
解决办法是，使用scrollTop来操作；然后监听页面的滚动事件；只要滚动了页面，就强制刷新一个滚动出的值；
```javascript
function getWin(attr) {
    return document.documentElement[attr] || document.body[attr];
}
//上面的函数获取元素的属性，解决clientHeight，scrollTop等的兼容性问题；
//获取鼠标滚动掉的像素;鼠标滚动的时候，强制刷新winTop;
var winTop =getWin("scrollTop");
window.onscroll=function(){
    winTop =getWin("scrollTop");
}
```

放大镜的镜片不透明在IE6的解决办法
用低不透明的png图片，然后再加一个边框；因为低不透明度的图片在在IE里不显示，直接为空的；只显示边框；这样也可以达到指定范围的效果；

浮动层的div被select遮挡
原因： 在IE中,select属于window类型控件，它会“挡住”所有非window类型控件 可以这么理解，div这样的组件是在浏览器客户区使用代码“渲染”的， 他们被渲染在客户区的绘画表面上， 而select是使用的标准windows控件，只是作为客户区的子控件放置而已， 它会覆盖所有客户区绘画表面上“画”出来的一切，但不一定会覆盖其他类型的window控件， 比如iframe和其他的select，如果你使用过类似Delphi这样的环境就会自然理解。IE7解决了此类BUG。

##### 解决办法：

-修改select，不用标准select，而是自己用其他html元素模拟
- 修改你的div，使用iframe。
- 在div被显示的时候或者到达select所在位置时隐藏select
- 在div中或div的同一坐标上，用相同尺寸的iframe先遮挡一下，然后在iframe上显示div的内容。 5.Object对象的优先度较高,可以挡住select框

最好的解决办法最好的方法：iframe来当作div的底 Div被Select挡住，是一个比较常见的问题。

有的朋友通过把div的内容放入iframe或object里来解决。

可惜这样会破坏页面的结构，互动性不大好。

这里采用的方法是：

虽说div直接盖不住select

但是div可以盖iframe，而iframe可以盖select,

所以，把一个iframe来当作div的底，

这个div就可以盖住select了.

原来的JS代码是：

```javascript
var bigImg=document.createElement("img");
bigImg.src="img/peony.jpg";
bigImg.id="bigImg";
container.appendChild(bigImg);
```
改后的是：

```javascript
var bigImg=document.createElement("img");
bigImg.src="img/peony.jpg";
bigImg.id="bigImg";
var pBigImg=document.createElement("p");
pBigImg.innerHTML="<iframe src=\"\" style=\"width:800px;height:536px;top:0px;left:0px;position:absolute;visibility:inherit;z-index:-1;\" frameborder=0></iframe>"
pBigImg.appendChild(bigImg);
container.appendChild(pBigImg);
```

做个人页面的时候，用雪碧图的时候，背景图片不出来；
原本代码：
```css
#introduces .left ul li .user{background: url("../img/ico.png")no-repeat 0 0;}

```
修改后的代码
```css
#introduces .left ul li .user{background: url("../img/ico.png") no-repeat 0 0;}

```
仅仅是相差了一个空格；在no-repeat前面打一个空格即可；

#### 经典padding兼容性问题；

IE低版本会把padding算成width的一部分；在做技能技能书的时候，如果用padding-left属性让文字向右；在地版本IE里就会有误差；解决的办法是可以用首行空格算；使用text-indent: 11px;即可；这个属性如果写text-indent: 2em;代表的就是坑2个字符；

当在一个容器里文字和img、input、textarea、select、object等元素相连的时候，对这个容器设置的line-height数值会失效；
对和文字相连接的img、input、textarea、select、object等元素加以属性
``` 
margin: (所属line-height-自身img,input,select,object高度)/2px 0; 
vertical-align:middle 
```

如果整个页面中有很多Img时，可以直接定义

```css
img{ 
    margin: (所属line-height-自身img,input,select,object高度)/2px 0; 
    vertical-align:middle 
}
```
网上也有人说是，line-height只可以应用于inline行内元素。所以line-height属性的设置对input元素是无效的。

```
display:block;    /*转换块级*/
display:inline;    /*转换行内*/
display:inline-block;    /* 其实仍未行内元素设置width及height属性等*/
```

但是我设置display:inline;还是不行；

因为我是需要一次性设置9张图片，所以没有好的解决办法；然后就直接改素材，把素材该大即可；

当一个css样式同时设置了float和margin的属性的时候，在ie7+及火狐上，该元素显示正常。但是在ie6下，将会出现双倍的margin属性值，
``` 
<div style="float:left;margin-left:10px;"> </div> 
```
上面的就是双倍的margin-left；解决办法是解决此办法的最简单的方法是，在style中添加：display:inline;
```
<div style="float:left;display:inline;margin-left:10px;"> </div> 
```
IE6中line-height的兼容性问题；
``` 
height:59px;
line-height: 60px;
```

上面的代码，在IE6中会被解析为，宽度是60px的；只需要把line-height改的和height一样就可以了；

margin-bottom在标准和IE不一致的时候（IE有时候会多一些像素），可以用display:inline-block转一下，然后统一设置，这样就可以了
display:inline-block 简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个link（a元素）inline-block属性值，使其既具有block的宽度高度特性又具有inline的同行特性。

文字两边对齐
用text-align即可；
```
text-align:justify;
```
