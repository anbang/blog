这个里面最常用的是一个属性就是：
```
-webkit-transition: 0.3s all ease;
-moz-transition: 0.3s all ease;
-ms-transition: 0.3s all ease;
```
HTML5和CSS3配合的；

DOM1的代码如下：

 

HTML部分；
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="style/move.css"/>
</head>
<body>
<p class="link">
    <a href="index1.html" class="active">DEMO1</a>
    <a href="index2.html">DEMO2</a>
</p>
<ul class="list">
    <li>
        <span class="icon">亚</span>
        <div class="text">
            <h2>关于我们</h2>
            <h3>金三胖是宇宙大战中，地球球长，宇宙总指挥！</h3>
        </div>
    </li>
    <li>
        <span class="icon">洲</span>
        <div class="text">
            <h2>联系我们</h2>
            <h3>金三胖的亚洲州长办公室位于地球，朝鲜半岛！</h3>
        </div>
    </li>
    <li>
        <span class="icon">洲</span>
        <div class="text">
            <h2>给我们钱</h2>
            <h3>最好给粮食！金三胖最爱吃板面，记得加个蛋！</h3>
        </div>
    </li>
    <li>
        <span class="icon">长</span>
        <div class="text">
            <h2>联系方式</h2>
            <h3>以45°角仰望天空，默念三胖你最屌，有机会收到感应！</h3>
        </div>
    </li>
    <li>
        <span class="icon">金</span>
        <div class="text">
            <h2>公司路线</h2>
            <h3>对P民无用，P民只能仰望三胖！</h3>
        </div>
    </li>
    <li>
        <span class="icon">三</span>
        <div class="text">
            <h2>加入我们</h2>
            <h3>宇宙大战，即将开始，请广大爱国者勇于加入！</h3>
        </div>
    </li>
    <li>
        <span class="icon">胖</span>
        <div class="text">
            <h2>准备战斗</h2>
            <h3>宇宙大战，即将开始，请广大爱国者勇于加入！</h3>
        </div>
    </li>
</ul>
<div class="broszhu">
    <h3 id="home_top">HTML5+CSS3实现菜单列表　-　Powered By <a href="http://taobao.fm/"target="_blank">broszhu</a>　　博客网址：<a href="http://taobao.fm/" target="_blank">taobao.fm</a> </h2>
        <p id="home_bottom">学习JavaScript是一件很有趣的事，可以做出很多自豪的小玩意！--broszhu （这个页面是实现原理：<a href="#" target="_blank">点此查看</a>）</p>
</div>
</body>
</html>
```
CSS如下：
```css
@charse
/*以下是清空样式*/
body, ul, ol, li, p, h1, h2, h3, h4, h5, h6, form, fieldset, table, td, img, div, dl, dt, dd, input {
    margin: 0;
    padding: 0;
    border: 0;
}
body {
    font-size: 12px;
    font-family: "Microsoft YaHei", 微软雅黑, "SimSun", "Arial Narrow", HELVETICA;
}
ul, ol,li {
    list-style-type: none;
}
select, input, img {
    vertical-align: middle;
}
a {
    text-decoration: none;
}
/*首页背景*/
body {
    background: url("../img/beige_paper.png") left top repeat;
}
/*顶部锚文本区*/
.link {
    padding-top: 10px;
    text-align: center;
}
.link a {
    display: inline-block;
    width: 51px;
    height: 20px;;
    line-height: 20px;
    margin: 0 5px;
    border: 1px solid #BFBFFF;
    border-radius: 3px;
    padding: 0 4px;
    background: #fff;
    color: #f30;
    font-size: 12px;
}
.link a.active {
    background: #259add;
    color: #fff;
}
/*UL列表页*/
.list {
    margin-top: 50px;
    width: 100%;
    height: auto;
}
.list li {
    width: 500px;
    height: 100px;
    margin: 0 auto;
    margin-bottom: 10px;
    background: #fff;
    box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
    transition: 0.3s all ease;
    overflow: hidden;
}
/*-webkit-transition;是动画出现的过度状态，0.3s all ease;代表0.3秒， 所有 安逸的*/
/*-webkit-是谷歌跟苹果浏览器的私有前缀；moz是火狐浏览器的私有前缀，ms是IE，o是欧鹏浏览器*/
.list li:hover {
    cursor:pointer;
    background: #E1F0FA;
    box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.4);
}
/*box-shadow是盒子阴影的意思，后面一共跟了四个数字，第一个是分别为X轴偏移量，Y轴偏移量，阴影半径；第四个是color；*/
/*rgba分别达标red，green，b色，o代表透明度，opacity*/
.list li:hover .text h2 {
    color: #259add;
    -webkit-animation: moveTop 300ms ease-in-out ;
    -moz-animation: moveTop 300ms ease-in-out ;
    -ms-animation: moveTop 300ms ease-in-out ;
    -o-animation: moveTop 300ms ease-in-out ;
}
.list li:hover .text h3 {
    color: #000;
    -webkit-animation: moveBottom 300ms ease-in-out;
    -moz-animation: moveBottom 300ms ease-in-out;
    -ms-animation: moveBottom 300ms ease-in-out;
}
/*图标的CSS3*/
.list li:hover .icon{
    color: #fff;
    font-size: 50px;
}
.list li .icon {
    color: #fff;
    width: 90px;
    height: 90px;
    margin: 5px 20px 0;
    line-height: 90px;
    text-align: center;
    font-size: 30px;
    background: #259add;
    display: inline-block;
    float: left;
    border-radius: 10px;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
    text-shadow: 0 0 3px #CCCCCC;
}
/*文字部分*/
.text {
    width: 350px;
    float: left;
    margin-top: 16px;
    height: 70px;
}
.text h2 {
    color: #999;
    font-size: 30px;
    font-weight: normal;
}
.text h3 {
    color: #666;
    font-size: 14px;
    font-weight: normal;
}
/*动画效果*/
@-webkit-keyframes moveTop {
    from {
        opacity: 0;
        -webkit-transform: translateY(-200%);
    }
    to {
        opacity: 1;
        -webkit-transform: translateY(0%);
    }
}
@-moz-keyframes moveTop {
    from {
        opacity: 0;
        -moz-transform: translateY(-200%);
    }
    to {
        opacity: 1;
        -moz-transform: translateY(0%);
    }
}
@-ms-keyframes moveTop {
    from {
        opacity: 0;
        -ms-transform: translateY(-200%);
    }
    to {
        opacity: 1;
        -ms-transform: translateY(0%);
    }
}
@-webkit-keyframes moveBottom {
    from {
        opacity: 0;
        -webkit-transform: translateY(200%);
    }
    to {
        opacity: 1;
        -webkit-transform: translateY(0%);
    }
}
@-moz-keyframes moveBottom {
    from {
        opacity: 0;
        -moz-transform: translateY(200%);
    }
    to {
        opacity: 1;
        -moz-transform: translateY(0%);
    }
}
@-ms-keyframes moveBottom {
    from {
        opacity: 0;
        -ms-transform: translateY(200%);
    }
    to {
        opacity: 1;
        -ms-transform: translateY(0%);
    }
}
/*链接部分*/
.broszhu{border: 1px solid #CCC;}
#home_top {height: 48px;line-height:48px;font-size:18; text-align: center; color:#008CD7;}
#home_bottom {color:#008CD7;text-align:center;font-size:14px;margin-bottom: 50px;}
```

DEMO2的如下：

HTML

 ```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="style/move2.css"/>
</head>
<body>
<p class="link">
    <a href="index1.html">DEMO1</a>
    <a href="index2.html" class="active">DEMO2</a>
</p>
<ul class="list">
    <li>
        <div class="border"></div>
        <span class="icon">亚</span>
        <div class="text">
            <h2>关于我们</h2>
            <h3>金三胖是宇宙大战中，地球球长，宇宙总指挥！</h3>
        </div>
    </li>
    <li>
        <div class="border"></div>
        <span class="icon">洲</span>
        <div class="text">
            <h2>联系我们</h2>
            <h3>金三胖的亚洲州长办公室位于地球，朝鲜半岛！</h3>
        </div>
    </li>
    <li>
        <div class="border"></div>
        <span class="icon">洲</span>
        <div class="text">
            <h2>给我们钱</h2>
            <h3>最好给粮食！金三胖最爱吃板面，记得加个蛋！</h3>
        </div>
    </li>
    <li>
        <div class="border"></div>
        <span class="icon">长</span>
        <div class="text">
            <h2>联系方式</h2>
            <h3>以45°角仰望天空，默念三胖你最屌，有机会收到感应！</h3>
        </div>
    </li>
    <li>
        <div class="border"></div>
        <span class="icon">金</span>
        <div class="text">
            <h2>公司路线</h2>
            <h3>对P民无用，P民只能仰望三胖！</h3>
        </div>
    </li>
    <li>
        <div class="border"></div>
        <span class="icon">三</span>
        <div class="text">
            <h2>加入我们</h2>
            <h3>宇宙大战，即将开始，请广大爱国者勇于加入！</h3>
        </div>
    </li>
    <li>
        <div class="border"></div>
        <span class="icon">胖</span>
        <div class="text">
            <h2>准备战斗</h2>
            <h3>宇宙大战，即将开始，请广大爱国者勇于加入！</h3>
        </div>
    </li>
</ul>
<div class="broszhu">
    <h3 id="home_top">HTML5+CSS3实现菜单列表　　-　Powered By <a href="http://taobao.fm/"target="_blank">broszhu</a>　　博客网址：<a href="http://taobao.fm/" target="_blank">taobao.fm</a> </h2>
        <p id="home_bottom">学习JavaScript是一件很有趣的事，可以做出很多自豪的小玩意！--broszhu （这个页面是实现原理：<a href="#" target="_blank">点此查看</a>）</p>
</body>
</html>
```
CSS代码如下：

```css
@charset "utf-8";
/*以下是清空样式*/
body, ul, ol, li, p, h1, h2, h3, h4, h5, h6, form, fieldset, table, td, img, div, dl, dt, dd, input {
    margin: 0;
    padding: 0;
    border: 0;
}
body {
    font-size: 12px;
    font-family: "Microsoft YaHei", 微软雅黑, "SimSun", "Arial Narrow", HELVETICA;
}
ul, ol,li {
    list-style-type: none;
}
select, input, img {
    vertical-align: middle;
}
a {
    text-decoration: none;
}
/*首页背景*/
body {
    background: url("../img/beige_paper.png") left top repeat;
}
/*顶部锚文本区*/
.link {
    padding-top: 10px;
    text-align: center;
}
.link a {
    display: inline-block;
    width: 51px;
    height: 20px;;
    line-height: 20px;
    margin: 0 5px;
    border: 1px solid #BFBFFF;
    border-radius: 3px;
    padding: 0 4px;
    background: #fff;
    color: #f30;
    font-size: 12px;
}
.link a.active {
    background: #259add;
    color: #fff;
}
/*UL列表页*/
.list {
    margin-top: 50px;
    width: 100%;
    height: auto;
}
.list li {
    width: 500px;
    height: 100px;
    margin: 0 auto;
    margin-bottom: 10px;
    background: #fff;
    box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
    transition: 0.3s all ease;
    overflow: hidden;
    position: relative;
}
.list .border{
    height: 100px;
    position: absolute;
    left: 0;top: 0;
    width: 10px;
    overflow: hidden;
    opacity: 0;
    background: #F90;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
}
.list li:hover {
    cursor:pointer;
    background: #000;
    box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.4);
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
}
.list li:hover .border{
    opacity: 1;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
}
.list li:hover .text h2 {
    color: #fff;
    font-size: 18px;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
}
.list li:hover .text h3 {
    color: #F60;
    font-size: 18px;
    margin-top: 10px;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
}
/*图标的CSS3*/
.list li:hover .icon{
    color: #F90;
    font-size: 50px;
    background: #000;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
}
.list li .icon {
    color: #fff;
    width: 90px;
    height: 90px;
    margin: 5px 20px 0;
    line-height: 90px;
    text-align: center;
    font-size: 30px;
    background: #259add;
    display: inline-block;
    float: left;
    border-radius: 10px;
    -webkit-transition: 0.3s all ease;
    -moz-transition: 0.3s all ease;
    -ms-transition: 0.3s all ease;
    text-shadow: 0 0 3px #CCCCCC;
}
/*文字部分*/
.text {
    width: 350px;
    float: left;
    margin-top: 16px;
    height: 70px;
}
.text h2 {
    color: #999;
    font-size: 30px;
    font-weight: normal;
}
.text h3 {
    color: #666;
    font-size: 14px;
    font-weight: normal;
}
/*链接部分*/
.broszhu{border: 1px solid #CCC;}
#home_top {height: 48px;line-height:48px;font-size:18; text-align: center; color:#008CD7;}
#home_bottom {color:#008CD7;text-align:center;font-size:14px;margin-bottom: 50px;}
```