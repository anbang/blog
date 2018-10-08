在标准浏览器中（IE10+）

用的是

``` 
var xhr=new XMLHttpRequest;
```
在IE大爷中，用的是ActiveXObject
```
//标准浏览器，IE10+
var xhr=new XMLHttpRequest;
//下面是IE的，分几个版本。一般写第一行的就可以了；
var _xhr1=new ActiveXObject("Microsoft.XMLHTTP");
var _xhr2=new ActiveXObject("Msxml2.XMLHTTP");
var _xhr3=new ActiveXObject("Msxml3.XMLHTTP");
```
AJAX兼容的写法，最简单的可以下面这样写：

```
function getXHR(){
    if("XMLHttpRequest" in this){
        return new XMLHttpRequest
    }
    if("ActiveXObject" in this){
        return new ActiveXObject("Microsoft.XMLHTTP")
    }
    return false;//容错用的
}
console.log(getXHR())
```
但是这种方法，每一次都需要判断一次；很浪费性能；

可以用惰性函数的原理来处理这个问题；

具体代码如下；
```
function getXHR() {
    for (var xhrlist = [function () {
        return new XMLHttpRequest();
    }, function () {
        return new ActiveXObject("Microsoft.XMLHTTP");
    }, function () {
        return new ActiveXObject("Msxml2.XMLHTTP");
    }, function () {
        return new ActiveXObject("Msxml3.XMLHTTP");
    }], i = 0, obj = null; i < xhrlist.length; i++😉
    {
        try {
            obj = xhrlist[i]();
            break;
        } catch (ex) {
            continue;//如果不写这一行，就不叫惰性函数了。本方法核心就是利用惰性函数；第一次计算得到的值，供内部函数调用；以后就不用计算了，也不用判断分支条件；这时函数就相当于一个被赋值的变量；
        }
    }
    if (!obj) {
        throw  new ReferenceError("this browser was no supported this featrue");
    }
    getXHR = xhrlist[i];
    return obj;
}
```
AJAX的四部曲的代码如下：
```
//AJAX 四部如下；
var xhr = getXHR();
xhr.open('get', '/ajax/yatao/index.html', true, void 0, void 0);//里面第四和第五项的'username','password' 如果不传。默认值是void 0，相当于undefined；
//有5种状态： 0 undent; 1 opened;2 header_received;3 loading;4 done
//只需要判断234就可以了，0和1不用管；
//如果写false就是同步，相当于买火车票时候只有一个售票窗口；true是异步执行，相当于好几个窗口；
/*    xhr.addEventListener("readystatechange",function(){//每改变一次，就会触发一次
 //console.log(this.readyState)
 if(this.readyState==4&&this.status==200){
 console.log(this.responseText);
 }
 });*/
xhr.onreadystatechange = function () {//兼容IE的写法；
    if (this.readyState == 4 && this.status == 200) {
        console.log(this.responseText);
    }
};
xhr.send();//可以传的类型。空着相当于undefined；可以传的项undefined null string blob formdata arraybuffer;
```
也可以像下面这么写：
```
var xhr = getXHR();
xhr.open('get', '/ajax', false, 'username', 'password');
// 0 unsent 1 opened 2 header_received 3 loading 4 done
xhr.onreadystatechange= function () {
    if(this.readyState==4){
        console.log(this.responseText);
    }
};
xhr.onload=function(){
    console.log(this.responseText);
};
xhr.onloadend=function(){
    console.log(this.responseText);
};
xhr.send('123');//undefined null string blob formdata arraybuffer
```
jQuery封装AjAX的思路如下：

 
```
//jQuery中封装AJAX的思路如下；
document.onreadystatechange=function(){
    if(document.readyState=='interactive'){
    }
};
document.addEventListener('DOMContentLoaded',function(){
});
```
—-

用nodeJS配合AJAX学习；
```
var http = require("http");
var fs = require('fs');
var url = require('url');
//读取服务器上的文件
var readFile=function(path,response){
  fs.readFile(path,function(err,data){
      if(err){
          //没找到，就返回404
          response.writeHead(404);
          return response.end("not found this file");
      }
      //已经找到，并且返回给浏览器
      response.writeHead(200);
      response.end(data)
  })
};
//利用node原生http模块，创建一个服务器
var server = http.createServer(function (request,response) {
    //格式化请求
    var urlobj=url.parse(request.url);
    //如果请求的是根目录或者/index.html就直接返回index.html
    if(urlobj.pathname=='/'||urlobj.pathname=="/index.html"){
        readFile("index.html",response);
        //如果请求的路径为/ajax 就返回一个文本
    }else if(urlobj.pathname=="/ajax"){
        response.writeHead(200,{"Content-Type":"text/plain"});//ascii
        response.end('hello world');
    }else{
        //上面两个条件不满足就直接读取请求的路径并返回
        readFile(urlobj.pathname.slice(1),response);
    }
});
server.listen(8080,function(){
    console.log("server start")
});
```