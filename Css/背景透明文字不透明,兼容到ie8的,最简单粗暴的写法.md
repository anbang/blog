今天工作中遇到一个需求，

是要实现“背景透明文字不透明”；

我开始是新建一个兄弟元素做的，后来想着用伪元素来做；

但是前端leader说伪元素也不好，让试试rgba，然后网上搜兼容IE方法；

看到网上很多种写法，都很麻烦，

很多教程和帖子是这种写法；

```
background:rgba(255, 255, 255, 0.6)!important;
filter:Alpha(opacity=60); background:#fff;
```
然后再这个子元素下面再让字体凸显出来；

比如；
```
.content { width:180px; height:260px; margin:0px auto; padding:30px 30px;background:rgba(255, 255, 255, 0.6)!important;
filter:Alpha(opacity=60); background:#fff; /*　使用IE专属滤镜实现IE背景透明*/ }
.content p{ position:relative;} /*实现IE文字不透明*/
```
~

这个需求如果用工具实现，真的是太方便了；

推荐一个网址：http://www.css88.com/demo/hex_color/

输入你需要实现的16进制颜色；然后输入需要做到的透明度多少；

然后生成的代码，不要直接用；

只有一行是针织有用的（把红色框内的代码，复制到需要的类或者ID中就可以了；）；

CSS的reba的背景透明,字体颜色不透明

这种方法的优势是，不需要带操作子元素；而且复杂的HTML结构中，操作子类我试了下，好像有点问题（也可能是我搜到的教程方法中不对）；

这个页面自动生成的代码，直接用下就可以了，简单粗暴有效果！、

IE亲测背景透明文字不透明

