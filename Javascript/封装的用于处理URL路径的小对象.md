URL路径是前端业务中，会碰到的一个东西；

下面的是一个处理URL的(为了演示方便，封装格式做了改变)；

里面有下面几个方法

1、getParam：参数是url中的key，用来获取指定key的value；

2、getQuery：参数是key[,path] ;可以在指定的url中查找指定key的value；如果不指定url，则在当前环境的URL中查找；

3、getUrlSearchParam：参数是url路径，返回格式化的query；

4、parseURL：这个是常用的方法，用来解析url的；参数是一个url；返回一个对象；

5、replaceUrlParams：参数是一个url，一个对象；用来替换路径中的同名参数；

DEMO演示：https://zhubangbang.com/demo/for-url/index.html

代码如下：
```javascript
window.Utility = {};
(function (URLObj) {
    //搜索参数
    URLObj.getParam = function (name) {
        var sUrl = window.location.search.substr(1);
        var r = sUrl.match(new RegExp("(^|&)" + name + "=([^&]*)(&|$)"));
        return (r == null ? null : unescape(r[2]));
    };
    
    URLObj.getQuery = function (name, url) {
        //参数：变量名，url为空则表从当前页面的url中取
        var u = arguments[1] || window.location.search.replace("&amp;", "&"),
            reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)"),
            r = u.substr(u.indexOf("\?") + 1).match(reg);
        return r != null ? r[2] : "";
    };
    
    //传一个路径，找的内容，以对象形式返回；
    URLObj.getUrlSearchParam = function (path) {
        var result = {}, param = /([^?=&]+)=([^&]+)/ig, match;
        while ((match = param.exec(path)) != null) {
            result[match[1]] = match[2];
        }
        return result;
    };
    //分析url
    URLObj.parseURL = function (url) {
        var a = document.createElement('a');
        a.href = url;
        return {
            source: url,
            protocol: a.protocol.replace(':', ''),
            host: a.hostname,
            port: a.port,
            query: a.search,
            params: (function () {
                var ret = {},
                    seg = a.search.replace(/^\?/, '').split('&'),
                    len = seg.length, i = 0, s;
                for (; i < len; i++) {
                    if (!seg[i]) {
                        continue;
                    }
                    s = seg[i].split('=');
                    ret[s[0]] = s[1];
                }
                return ret;
            })(),
            file: (a.pathname.match(/\/([^\/?#]+)$/i) || [, ''])[1],
            hash: a.hash.replace('#', ''),
            path: a.pathname.replace(/^([^\/])/, '/$1'),
            relative: (a.href.match(/tps?:\/\/[^\/]+(.+)/) || [, ''])[1],
            segments: a.pathname.replace(/^\//, '').split('/')
        };
    };
    
    //替换myUrl中的同名参数值
    URLObj.replaceUrlParams = function (myUrl, newParams) {
        /*
        for (var x in myUrl.params) {
            for (var y in newParams) {
                if (x.toLowerCase() == y.toLowerCase()) {
                    myUrl.params[x] = newParams[y];
                }
            }
        }
        */
        for (var x in newParams) {
            var hasInMyUrlParams = false;
            for (var y in myUrl.params) {
                if (x.toLowerCase() == y.toLowerCase()) {
                    myUrl.params[y] = newParams[x];
                    hasInMyUrlParams = true;
                    break;
                }
            }
            //原来没有的参数则追加
            if (!hasInMyUrlParams) {
                myUrl.params[x] = newParams[x];
            }
        }
        var _result = myUrl.protocol + "://" + myUrl.host + ":" + myUrl.port + myUrl.path + "?";
        for (var p in myUrl.params) {
            _result += (p + "=" + myUrl.params[p] + "&");
        }
        if (_result.substr(_result.length - 1) == "&") {
            _result = _result.substr(0, _result.length - 1);
        }
        if (myUrl.hash != "") {
            _result += "#" + myUrl.hash;
        }
        return _result;
    }
})(window.Utility);
    
// var myURL = Utility.parseURL('http://abc.com:8080/dir/index.html?id=255&m=hello#top');
// console.log(myURL);
```
下面是DEMO的演示

```javascript
// ********************* 下面都是DEMO *************************
    
//辅助输出
function w(str) {
    document.write(str + "<br>");
}
    
//parseURL
var urlStr = "http://abc.com:8080/dir/index.html?id=255&m=hello#top";
var myURL = Utility.parseURL(urlStr);
console.log(myURL);
    
w("myUrl.source = " + myURL.source);   // = 'http://abc.com:8080/dir/index.html?id=255&m=hello#top'
w("myUrl.file = " + myURL.file);     // = 'index.html'
w("myUrl.hash = " + myURL.hash);     // = 'top'
w("myUrl.host = " + myURL.host);     // = 'abc.com'
w("myUrl.query = " + myURL.query);    // = '?id=255&m=hello'
w("myUrl.params = " + myURL.params);   // = Object = { id: 255, m: hello }
w("myUrl.path = " + myURL.path);     // = '/dir/index.html'
w("myUrl.segments = " + myURL.segments); // = Array = ['dir', 'index.html']
w("myUrl.port = " + myURL.port);     // = '8080'
w("myUrl.protocol = " + myURL.protocol); // = 'http'
    
//replaceUrlParams
var _newUrl = Utility.replaceUrlParams(myURL, { id: 101, m: "World", page: 1, "page": 2 });
w("<br>新url为：");
w(_newUrl); //http://abc.com:8080/dir/index.html?id=101&m=World&page=2#top
    
//getParam
// 备注：本html打开的路径是  file:///E:/blog/zhubangbang/index.html?id=12345678
w("Utility.parseURL：" + Utility.getParam("id"));               //12345678
w("Utility.getQuery：" + Utility.getQuery("id", urlStr));        //255    从目标url中取；
w("Utility.getQuery：" + Utility.getQuery("id"));                //12345678 从当前取值
w("Utility.getUrlSearchParam：参见console");  // 
console.log("Utility.getUrlSearchParam(urlStr)", Utility.getUrlSearchParam(urlStr));
```
