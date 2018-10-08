HTTP method(HTTP方法)：

get :从服务器获取数据；

    1、有大小限制（因为他把数据全部放在URL上的。chrome的URL最大8K，FF7k，IE2k）；
    
    2、get没有请求主体的,请求主体为空；
    
    3、容易被缓存

4、所有条件都拼接到URL后面

post：往服务器发送数据

    1、没有大小限制
    
    2、把数据放到请求主体里了
    
    3、不会被缓存

put：往服务器推送数据（属于post系）

    特点和post一样

delete：删除服务器上的一个资源（属于get系，特点和get一样）
    
    head：只得到响应头信息（属于get系，特点和get一样）
    
    下面三个是没有用，忽略不计，混个脸熟就可以了；
    
    connection：链接
    
    trace：断言
    
    track：调试
    
    options：看服务器支持哪些http method？

 

既然get/post可以完成大多数事情，为啥还做这么多呢？

restful：表征状态转移；特定的东西干特定的事；

增删改查：put、delete、post、get；

增删改查的学术名是：CURD

http默认端口是80，端口号最大是65535；
https端口默认是443；


AJAX的四部如下：

 
``` 
function getXHR(){
    if(window.XMLHttpRequest){
        return new XMLHttpRequest();
    }
    return new ActiveXObject('Microsoft.XMLHTTP');
}
//1、获取AJAX实例；
var instance=getXHR();
/*
* 2、打开这个ajax对象
* 参数；http方法、请求服务器路径，是不是异步（默认是true）。url里面的username（默认是undefined）。url里面的possword（默认是undefined）
* */
instance.open('get','/url',true,'username','possword');//2、打开这个ajax对象；
//3、把参数放在请求主体里；如果用get不需要写；因为get的请求主体是空的；
instance.send('');
//4、每当readystate改变的时候，触发这个这个函数,负责接收；
/*
* 0、unsent 未发送，实例化的时候状态为这个；
* 1、opened 打开实例，调用open方法之后，状态改变为这个；
* 2、header-received 接收到服务器发送过来的响应头；
* 3、loading 响应首部接收完毕，开始接受响应主体；
* 4、done 全部完成
* 只有完成的时候才操作的；
* */
instance.onreadystatechange=function(){
    if(this.readyState==4&&this.status==200){//this.readyState==4或者用this.readyState==this.DONE
        console.log(this.response);//有兼容性；低版本IE不支持；IE8以下；
        console.log(this.responseText);
        console.log(this.responseXML);
    }
}
```
`上面的this DONE有兼容性，低版本不支持；

实际调用如下：
```
<script>
    function getXHR(){
        if(window.XMLHttpRequest){
            return new XMLHttpRequest();
        }
        return new ActiveXObject('Microsoft.XMLHTTP');
    }
    var instance=getXHR();
    instance.open('get','getData.js',true,'username','possword');
    instance.send('');
    instance.onreadystatechange=function(){
        if(this.readyState==4&&this.status==200){
            console.log("response: "+this.response);//有兼容性；低版本IE不支持；IE8以下；
            console.log("responseText: "+this.responseText);
            console.log("responseXML: "+this.responseXML);
        }
    };
    /****输出的结果如下；***
     response: [{"name":"test","age":"12"},{"name":"test","age":"12"},{"name":"test","age":"12"}]
     responseText: [{"name":"test","age":"12"},{"name":"test","age":"12"},{"name":"test","age":"12"}]
     responseXML: null
    * */
</script>
```
