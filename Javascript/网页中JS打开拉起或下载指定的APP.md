点击呼起APP非常简单，和打电话类似(点击打电话是a标签的：`tel:1xxxxxxx`)
```html
<a href="weixin://scanqrcode" id="open-weixin">微信</a>
```
如果想直接打开，修改href就好
```javascript
window.location.href = "weixin://";
```
还有一些下载的，做个备份；（没有测试的）
```javascript
    //生成一个url scheme,假设我们约定的scheme是myApp://type=1&id=iewo212j32这种形式的
    var baseScheme = "weixin://";
    var createScheme=function(options){
        var urlScheme=baseScheme;
        for(var item in options){
            urlScheme=urlScheme+item + '=' + encodeURIComponent(options[item]) + "&";
        }
        urlScheme = urlScheme.substring(0, urlScheme.length - 1);
        return encodeURIComponent(urlScheme);
    };
     
    //实际上就是新建一个iframe的生成器
    var  createIframe=(function(){
        var iframe;
        return function(){
            if(iframe){
                return iframe;
            }else{
                iframe = document.createElement('iframe');
                iframe.style.display = 'none';
                document.body.appendChild(iframe);
                return iframe;
            }
        }
    })();
     
    var openApp=function(){
        var localUrl=createScheme();
        //var localUrl=baseScheme;
        var openIframe=createIframe();
        if(UA.isIPhone){
            window.location.href = localUrl;
            var loadDateTime = Date.now();
            setTimeout(function () {
                var timeOutDateTime = Date.now();
                if (timeOutDateTime - loadDateTime < 1000) {
                    window.location.href = "http://www.qq.com/";
                }
            }, 1000);
        }else if(UA.isAndroid){
            if (UA.isChrome) {
                //chrome浏览器用iframe打不开得直接去打开，算一个坑
                window.location.href = localUrl;
            } else {
                //抛出你的scheme
                openIframe.src = localUrl;
            }
            setTimeout(function () {
                window.location.href = "https://www.baidu.com/";
            }, 500);
        }else{
            //主要是给winphone的用户准备的,实际都没测过，现在winphone不好找啊
            openIframe.src = localUrl;
            setTimeout(function () {
                window.location.href = "https://www.taobao.com/";
            }, 500);
        }
    };
```

一些参考的链接：

http://www.jianshu.com/p/94425b560ca4

http://www.openinstall.io/content.html

http://www.dcloud.io/docs/api/zh_cn/runtime.html （这个是打开默认浏览器的参考）