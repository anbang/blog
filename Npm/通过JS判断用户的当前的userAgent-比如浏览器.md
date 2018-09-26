于是在自己以前的校验中，又更新了一些浏览器判断；

在线检测，点开后就可以看到你当前浏览器的状况了；

https://zhubangbang.com/demo/get-ua/index.html

备注：欧朋已经用webkit的内核了，以后写CSS3也不需要加-o了（以前加-o是为了向后兼容，为的是适应以后的欧朋，没想到欧朋直接上了webkit的内核，然后就没有然后了）

代码如下；

``` 
(function getUA(window) {
    var ua     = navigator.userAgent.toLowerCase();
    var ieMode = document.documentMode,
        isIE   = !!window.ActiveXObject,
        isIE6  = isIE && !window.XMLHttpRequest;
    var isEdge=/edge/i.test(ua),
        isOpera=/opr/i.test(ua);
    var isIpad = /(ipad).*os\s([\d_]+)/i.test(ua);
 
    window.UA = {
        isMobile    : /applewebkit.*mobile.*/.test(ua) || /applewebKit/.test(ua),                           //是否为移动端
        isMac       : /mac os x/.test(ua),                                                                  //苹果电脑
        isAndroid   : ua.indexOf('android') > -1 || ua.indexOf('linux') > -1,                               //android终端
        isIPhone    : !isIpad && /(iphone\sos)\s([\d_]+)/i.test(ua),                                        //iphone
        isIPad      : isIpad,                                                                               //ipad
        isIos       : isIpad || /(iphone\sos)\s([\d_]+)/i.test(ua),                                         //ios系统,包括ipad和iphone；(不包含iPod touch)
        isWeiXin    : /micromessenger/i.test(ua),                                                           //weixin
        isUC        : ua.indexOf('ucbrowser') > -1,                                                         //UC
        isUC_Webkit : /uc\sapplewebkit\/([\d.]+)/i.test(ua),                                                //isUC_Webkit
        isUC_Proxy  : /(ucweb)(\d.+?(?=\/))/i.test(ua),                                                     //isUC_Proxy
 
        isWeibo     : /weibo/i.test(ua),                                                                    //在新浪微博客户端打开
        isQQ        : /(qq)\//i.test(ua),                                                                   //在QQ
        isChrome    : (/chrome\/([\d.]+)/.test(ua) || /crios\/([\d.]+)/.test(ua))&& !isEdge && !isOpera,    //Chrome
        isMozilla   : ua.indexOf('gecko') > -1 && ua.indexOf('khtml') == -1,                                //火狐内核
        isWebkit    : /applewebkit/i.test(ua),                                                              //苹果，谷歌内核
        isOpera     : isOpera,                                                                              //opera浏览器，webkit
        isSafari    : /safari/i.test(ua) && (!/chrome/i.test(ua)),                                          //苹果浏览器
        isBlackberry: /(blackberry).*version\/([\d.]+)/i.test(ua),                                          //blackberry
 
 
        isEdge      : isEdge,                                                                               //edge
        isIE        : isIE,                                                                                 //IE
        isIE6       : isIE6,                                                                                //IE6
        isIE7       : isIE && !isIE6 && !ieMode || ieMode == 7,                                             //IE7
        isIE8       : isIE && ieMode == 8,                                                                  //IE8
        isIE9       : isIE && ieMode == 9,                                                                  //IE9
        isIE10      : isIE && ieMode == 10,                                                                 //IE10
 
        is360mse    : /360 aphone browser|qhbrowser/i.test(ua),                                             //360手机浏览器
 
        isHorizontal: window.orientation == 90 || window.orientation == -90,                                //是否横屏
        isVertical  : window.orientation == 0 || window.orientation == 180                                  //是否竖屏
    }
})(window);
```

我自己测试了下，还是可以的；

脚本地址：https://zhubangbang.com/ssl/laboratory/get-ua.js

使用演示：


``` 
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<script src="https://zhubangbang.com/ssl/laboratory/get-ua.js"></script>
<script>
    if(UA.isWeiXin){
        console.log("现在是微信打开");
    }else  if(UA.isChrome){
        console.log("现在是Chrome打开");
    }else{
        console.log("other");
    }
</script>
</body>
</html>
```

有问题欢迎留言
