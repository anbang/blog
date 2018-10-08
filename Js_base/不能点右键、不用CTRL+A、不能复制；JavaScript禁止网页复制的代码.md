以前我做禁止复制的时候，用的是CSS的方法；而这个方法也比较low；就是禁止谷歌内核的；
``` 
<style>
    body{
        -webkit-user-select: none;
    }
</style>
```


这个只能禁止谷歌的；

然后网上搜了下做法；发现还是有很多的；

第一个禁止复制：
```
 <!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        /*页脚的标注*/
        .broszhu{margin: 90px 0 0 30px;}
        #home_top {height: 48px;line-height:48px;font-size:18; text-align: center; color:#008CD7;}
        #home_bottom {color:#008CD7;text-align:center;font-size:14px;}
    </style>
</head>
<body style="text-align:center">
<p> </p>
<p> </p>
<p>网页禁止右键、禁止查看源代码、禁止复制的代码，试试你的右键、ctrl+c和ctrl+c吧~
    <script>
        document.oncontextmenu=new Function('event.returnValue=false;');
        document.onselectstart=new Function('event.returnValue=false;');
    </script>
</p>
<div class="broszhu">
    <h3 id="home_top">404CryPage　-　Powered By
        <a href="http://taobao.fm/"target="_blank">broszhu</a>
        　　博客网址：<a href="http://taobao.fm/" target="_blank">taobao.fm</a>
    </h3>
    <p id="home_bottom">学习JavaScript是一件很有趣的事，可以做出很多自豪的小玩意！--broszhu （这个页面是实现原理：<a href="#" target="_blank">点此查看</a>）</p>
</div>
</body>
</html>
```
这个方法亲测IE是没问题的；

还有一种是放在body里面的标签中；

亲测IE谷歌等也是可以用的；
```
 <!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        .div1{
            width: 900px;
            height:300px;
            margin: 0 auto;
        }
        /*页脚的标注*/
        .broszhu{margin: 90px 0 0 30px;}
        #home_top {height: 48px;line-height:48px;font-size:18; text-align: center; color:#008CD7;}
        #home_bottom {color:#008CD7;text-align:center;font-size:14px;}
    </style>
</head>
<body oncontextmenu="window.event.returnValue=false"
      onkeypress="window.event.returnValue=false"
      onkeydown="window.event.returnValue=false"
      onkeyup="window.event.returnValue=false"
      ondragstart="window.event.returnValue=false"
      onselectstart="event.returnValue=false">
<div class="div1">
    <p>禁止网页复制的代码如下</p>
    <p>只需要在body标签里面加入下面的代码就可以了</p>
    <p>oncontextmenu="window.event.returnValue=false"</p>
    <p>onkeypress="window.event.returnValue=false"</p>
    <p>onkeydown="window.event.returnValue=false"</p>
    <p>onkeyup="window.event.returnValue=false"</p>
    <p>ondragstart="window.event.returnValue=false"</p>
    <p>onselectstart="event.returnValue=false"</p>
</div>
<div class="broszhu">
    <h3 id="home_top">禁止复制的网页代码如下　-　Powered By
        <a href="http://taobao.fm/"target="_blank">broszhu</a>
        　　博客网址：<a href="http://taobao.fm/" target="_blank">taobao.fm</a>
    </h3>
    <p id="home_bottom">学习JavaScript是一件很有趣的事，可以做出很多自豪的小玩意！--broszhu （这个页面是实现原理：<a href="http://taobao.fm/archives/1254" target="_blank">点此查看</a>）</p>
</div>
</body>
</html>
```

网上找到的总结如下：

 



1、使右键和复制失效

方法1：

在网页中加入以下代码：
```
<script language=”Javascript”>
document.oncontextmenu=new Function(“event.returnValue=false”);
document.onselectstart=new Function(“event.returnValue=false”);
</script>
```
方法2：

在<body>中加入以下代码：
```
<body oncontextmenu=”return false” onselectstart=”return false”>
```
或
```
<body oncontextmenu=”event.returnValue=false” onselectstart=”event.returnValue=false”>
```
实质上，方法2与方法1是一样的。

方法3：
如果只限制复制，可以在<body>加入以下代码：
```
<body oncopy=”alert(‘对不起，禁止复制！’);return false;”>
```
2、使菜单”文件”－”另存为”失效

如果只是禁止了右键和选择复制，别人还可以通过浏览器菜单中的”文件”－”另存为”拷贝文件。为了使拷贝失效，可以在<body>与</body>之间加入以下代码：
```
<noscript>
<iframe src=”*.htm”></iframe>
</noscript>
```
这样，用户在另存网页时，就会出现”无法保存Web页”的错误。