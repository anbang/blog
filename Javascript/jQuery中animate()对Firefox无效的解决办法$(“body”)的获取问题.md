很久没有写官网页面了；最近接一个新需求，做一个区块链的官网宣传页；

兼容主流浏览器即可；

我做的兼容到 chrome，IE7、火狐浏览器

使用jQuery的时候遇到一个事情；就是animate的时候，竟然在animate上无效；查找后发现是DOM获取的问题；记录下；

在使用 animate（）做返回顶部的动画时，会出现对Firefox无效的情况;

开始是下面这种写法的：
```
    $(“body”).stop().animate({ scrollTop: targetScrollVal-100 }, 500,"swing");
```
这种在IE和chrome下都是没问题的；

但是这么写在火狐下是无效的；

火狐下是这么写的

```
    $(“html”).stop().animate({ scrollTop: targetScrollVal-100 }, 500,"swing");

```
两者结合后就可以了；

如下；

```
    var $page=$("#page-canon-index");
    var $body=$("html,body");
    var $linkWrap=$page.find("#header-link-wrap");
    var targetScrollVal;//储存需要跳转到的目标值；
     
    var pageUtility={
        init:function () {
            this.bind();
        },
        bind:function () {
            var self=this;
            $linkWrap.on("click",".link-item",function (e) {
                e.preventDefault();
                var indexVal=$(this).closest("li").data("index");
                self.floorSwitch(indexVal);
            });
        },
        floorSwitch:function (indexVal) {
            targetScrollVal=$page.find("#page"+indexVal)[0].offsetTop;
            $body.stop().animate({ scrollTop: targetScrollVal-100 }, 500,"swing");
        }
    };
    pageUtility.init();
```

这就是全部的写法；

核心代码：

```
    var $body=$("html,body");
``` 