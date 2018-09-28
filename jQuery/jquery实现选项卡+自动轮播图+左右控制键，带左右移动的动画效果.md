jquery实现的方法；精简了几次，感觉写的已经没有BUG了，代码也蛮精简的；

```
    /*做的DEMO中一共有5个选项卡；
    *#banner是整个banner区域；包含banner-tittle；banner-body；
    *banner-body：是选项卡需要控制的内容包裹层；
    *banner-left-right 是左右控制器的包裹层；
    * */
```

DOM结构如下：

```html
<!--banner区域开始-->
<div id="banner" class="">
    <!--banner-tittle用来做轮播的控制头-->
    <ul class="banner-tittle">
        <li class="select"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
    <div class="banner-body">
        <div class="banner-body-none">
            <a href="javascript:;">
                <img src="img/index-banner.jpg" alt="杭州微飞胜科技有限公司-banner1"/>
            </a>
            <!--<a href="javascript:;" class="banner1-button debug"><p>马上加入</p></a>-->
        </div>
        <div class="banner-body-none">
            <a href="javascript:;">
                <img src="img/index-banner2.jpg" alt="杭州微飞胜科技有限公司-banner1"/>
            </a>
        </div>
        <div class="banner-body-none">
            <a href="javascript:;">
                <img src="img/index-banner3.jpg" alt="杭州微飞胜科技有限公司-banner1"/>
            </a>
        </div>
        <div class="banner-body-none">
            <a href="javascript:;">
                <img src="img/index-banner4.jpg" alt="杭州微飞胜科技有限公司-banner1"/>
            </a>
        </div>
        <div class="banner-body-none">
            <a href="javascript:;">
                <img src="img/index-banner5.jpg" alt="杭州微飞胜科技有限公司-banner1"/>
            </a>
        </div>
    </div>
    <div class="banner-left-right">
        <div id="banner-left">
            <span class="iconfont">&#58897;</span>
        </div>
        <div id="banner-right">
            <span class="iconfont">&#58898;</span>
        </div>
    </div>
</div>
<!--banner区域结束-->

```
JavaScript代码如下；

默认已经载入jQuery；

 

```javascript
var _index=0;
var time = 0;
var $first=$(".banner-body div:first").clone(true);
var $last=$(".banner-body div:last").clone(true);
var $bannerBody = $(".banner-body");
    $banner = $("#banner")
    $bannerLeftRight = $(".banner-left-right")
    $bannerTitLi = $(".banner-tittle li");
var $bannerR = $("#banner-right"),
    $bannerL = $("#banner-left");

$bannerBody.append($first); 
$bannerBody.prepend($last);
$bannerBody.css("left","-1920px");
//鼠标放进去后停止动画；
$banner.hover(function() {
    clearInterval(time);
}, function() {
    time = setInterval("junmperRight()", 3000);
});
//鼠标移入，显示向左，向右的按钮；
$banner.hover(function() {
    $bannerLeftRight.fadeIn(300);
}, function() {
    $bannerLeftRight.fadeOut(300);
});
console.log($bannerLeftRight.fadeOut());
$bannerTitLi.click(function(){
    clearInterval(time);
    $(this).addClass("select").siblings().removeClass("select");
    $bannerBody.animate({'left':-1920*($(this).index()+1)+'px'},500);
    _index=$(this).index();
    });
    
//左右的按钮的事件；
$bannerL.click(function() {
    clearInterval(time);
    junmperLeft();
});
$bannerR.click(function() {
    clearInterval(time);
    junmperRight();
});
    
//动画效果
function junmperRight() {
    _index++;
    if (_index > 4){
        $bannerBody.animate({'left':-1920*(_index+1)+'px'},500,function(){
            $bannerBody.css('left','-1920px')
        });
        _index = 0;
        $bannerTitLi.eq(_index).addClass("select").siblings().removeClass("select");
    }else{
        $bannerTitLi.eq(_index).addClass("select").siblings().removeClass("select");
        $bannerBody.animate({'left':-1920*(_index+1)+'px'},500);
    }
}
function junmperLeft(){
    _index--;
    if (_index < 0){
        $bannerBody.animate({'left':-1920*(_index+1)+'px'},500,function(){
            $bannerBody.css('left','-9600px')
        });
        $bannerTitLi.eq(_index).addClass("select").siblings().removeClass("select");
        _index = 4;
    }else{
        $bannerTitLi.eq(_index).addClass("select").siblings().removeClass("select");
        $bannerBody.animate({'left':-1920*(_index+1)+'px'},500);
    }
}
time = setInterval("junmperRight()", 3000);

```
总结：

1、开始写的是直接让现实和隐藏，写好后，发现效果非常生硬； 

2、然后想着动态的改变DOM结构；DOM结构默认是01234这四个元素；先改变成40123这样的；默认显示0；改变一个就变一下DOM

```
    4【0】123 默认的初始状态显示第0个元素
    0【1】234
    1【2】340
    2【3】401
    3【4】012
    大括号内的当前显示的元素；
    写完后，感觉这种的效率太低了；
```

3、改变元素的位置来控制显示；【最终版】

- 1、先把元素的DOM结构首尾都复制，HTML结构是01234；默认编程【4】01234【0】，把banner-body的元素从5个变成7个；
- 2、根据index的控制需要向左偏移的数值；（当在第4个元素时候，快速把banner-body的位置拉到正确位置；前后同理；具体见源码；）
