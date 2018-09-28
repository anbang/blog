node+文件夹名可以以node的形式运行程序；和在webstrom中的运行一样（文件夹名不带后缀也可以运行。

比如aaaa.js或者aaaa都可以的）！

node的基础知识点；

- 一是模块化概念，；在模块化之上还有包的概念；
- 二是包的概念（package）；包的大于模块化的；

模块化的概念：module：//模块；用的是require做模块化；应用如下；

```javascript
event=require("event.js");//引用函数的；函数内需要暴露；
module.exports={proceThis:proceThis,on:on,off:off};//用这个在JS内暴露；
```

写一个node最基本的小框架；

```javascript
var http=require("http");
var fs=require("fs");//IO
var url=require("url");//解析URL
var querysting=require("querystring");//解析字符串查询；
http.createServer(function (req, res) {//请求和应答两个参数；
    //req是request的缩写，从浏览器端过来的请求；
    //res是response缩写，从服务器返回浏览器端的对象；
    res.writeHead(200);
    res.write('<!DOCTYPE html> <html> <head lang="en"><meta charset="UTF-8"> <title>node知识</title> </head> <body>');
    res.write(" <h1>学习nodejs</h1> ");//应答；
    res.write("这是我的第一个nodejs小程序");
    res.end("</body></html>")
}).listen(8081);
```
启动程序后，可以在8081端口查看；

### 改写代码

```javascript
var http=require("http");
var fs=require('fs');
http.createServer(function(request,response){
    getFile(request.url.slice(1),response);//需要处理路径
    function getFile(filename,response){
        fs.readFile(filename,function(err,data){
            if(data==null||err){
                response.end("file is undefind");
            }else{
                response.setHeader('Content-Type',getContentType(filename)+';charsrt=UTF-8');
                response.write(data);
                response.end();
            }
        })
    }
    function getContentType(filename){
        if(filename.indexOf('.html')!=-1){
            return 'text/html'
        }else if(filename.indexOf('.css')!=-1){
            return 'text/css'
        }else if(filename.indexOf('.js')!=-1){
            return 'text/javascript'
        }
    }
}).listen(8080);
```

### 请求和响应的常用方法

请求
```
request.url 获取路径
request.method
request.headers
Connection:keep-alive 保持连接；相当于保持连接不中断；
Content-Length  : 请求的长度；Content-Length:19   name=broszhu&&age=6;请求的长度也会影响采取何种请求方式；
浏览器只能发get请求，不能发表post请求；?????
```

响应：

```
response.write(‘hello’) 写文件
response.end() 响应结束
response.end(new Date(),toUTCString()); ????
response.statusCode=404 设置响应吗
response.setHeader(‘Content-Type’,’text/html;charset=”UTF-8”’)
response.writeHeade(200,{‘Content-Length’: data.length,‘Content-Type’;’text/html;charset=”UTF-8”’})
```

##### 提示下面报错

`Can’t set headers after they are sent.`

如果header已经发给客户端，那么就不能再设置

##### 转码：

- encodeURIComponent() 汉字转成编码;它是将中文、韩文等特殊字符转换成utf-8格式的url编码，所以如果给后台传递参数需要使用encodeURIComponent时需要后台解码对utf-8支持
- decodeURIComponent() 编码变转成汉字

##### url.parse

- url.parse(request.url)        —>node提供的parse方法；
- url.parse(request.url,true)   —>把查询字符串转成对象；

##### FS的方法：

```
fs.readFileSync(‘index2.html’,’UTF-8′) 按照UTF-8读取index2.html
```

##### URL方法：
```
url.parse(request.url,true).pathname    //请求路径”/” “/clock”带斜杠的；
```