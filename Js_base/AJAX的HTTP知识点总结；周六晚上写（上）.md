异步JavaScript和XML，是指一种创建交互式网页应用的网页开发技术。

AJAX=异步JavaScript和XML（标准通用标记语言的子集）；是一个前端技术，不是一个新的语言，是利用浏览器提供操作HTTP的接口（XMLHttpResquest或者ActiveXObject）来操作HTTP以达到异步的效果；

HTTP：是事务，包含【起始行】，【首部】，【主体】；


 

包括请求起始行和响应起始行；

请求起始行：请求方式，URL，版本号；例子：GET：/  HTTP/1.1

KV是key：value，也就是键值对的简写，也叫应映,?yingshe（map）；

首部（Headers）

通用：请求响应都有；Cache-Control在请求响应都有，304的原理（这个原理以后需要重点看）

请求：只有请求有；cookie（cookie只在请求头出现）登陆信息储存在这里；Host只有在请求头出现；

响应：server只在响应里出现；

主体：相当于集装箱，存储数据；get的方式，主体为空；post有主体；get从服务器拉取数据的；没有向服务器发送数据；所以主体是为空的；而post往服务器发数据；post为传数据；Response选项卡里就是主体；

请求：get主体为空，post主体为传输数据；
响应：都有主体；
HTTP知识扩展；

http method：有哪些方式？
```
get：从服务器拉取；（所以请求主体为空，因为get不提交数据；get有大小限制；在chrome下为8K；在FF为7K；在IE为2K；请求主体为空，会被缓存读取；后面加个随机数，可以让get不缓存；也可以加时间戳；）
post：往服务器发送；没有大小限制；不会被缓存；请求主体是有内容的；
delete：告诉服务器删除一个资源；是get系的；
head：服务器只返回一个响应头；get系；只有起始行和首部；
put：往服务器上推送资源；post系；
trace：
track：断言调试；
connection：链接服务器；
option：
```

思考：既然get和post可以完成所有的，为啥细分这么多；？？？设计者设计的时候：restful：因为表征状态转移；

时间戳的方法：1970年1月1日午夜；

 

restful API
```
//距离1970年1月1日午夜12点多少秒；
(new Date).getTime();
Date.now();
(new Date()).valueOf();
+(new Date());//相当于转型；+是一个技巧，转型数字用的；
```

CUKD：代表增删改查；对应的是：post 、delete、get、put；

angularJS就封装了restfulAPI（前端封）；

HTTP Status Code：

    1XX：1开头的已经取消了，只有一个还有1开头的，就是ES5的websocket（101）；
    2XX：常见的是200,202；HTTP事物代表成功了；
    3XX：常见的是301,302,303,304,307几种方法；
    3XX：301（永久重定向），302（临时重定向），303（临时转移），307（临时服务器重定向），304（缓存）；
    4XX：
    5XX：
    
负载均衡用303,307；服务器集群才用的；

301会降低网站权重；除非域名永久不用；否则不用；

304缓存：

    last-modifyed：响应头最后一次修改时间；是UTC时间；
    etag：翻译过来的实体标签；存散数列；通过sha1算法；
还有一组是

    if-modified-sine：请求头；时间；
    if-none-match：三劣质；
这个和last-modifyed，etag对应；

    cache-contorl：存的是时间，修饰符有public，private；修饰符可加可不加的；
    expires：储存的是秒（s）；
4XX：常见的有下面几种；

    400：请求缺少参数；
    401：未认证；
    403：禁止； forbidden就是403；base认证；
    404：未找到
    407：长传的资源过大；
    5XX：常见的就是500；属于服务器的事；与前端的代码是没关系的；不是由于前端代码引起错误的；

互联网公司岗位简称：FE是前端，RD是后端开发，QA是测试，UE和UI是设计，PM是产品经理，HR是人事；

MIME Type：告诉浏览器如何处理这个资源；

content-type：text/html；前面是大类型，后面的小类型；如text/CSS；

查看浏览器支持类型，可以在浏览器的控制台上述这段代码：navigator.mimeTypes；

常用的小技巧；
```
//转型；字符串转为数字；
var s="3.14";
//有以下9种方法；
s-0;
s*1;
s/1;
Number(s);
parseFloat(s);
parseInt(s);
s.valueOf();
Math.floor(s);
+(+s).toFixed(0);
```
保留小数点后的一位数字；
```
//小数点后保留一位有效数字；
var a=123.12333333;
//方法如下；
a.toFixed(1);//123.1;参数传的是保留几位数字；
a.toFixed(0);//123;

var a={};
Object.prototype.toString.call(a);
//结果是 "[object Object]"
toString.call(a);
//可以这么写；但是IE不兼容；
```
基础小知识回忆：

Js处理最大值的2的53次方减1；表达式是：
```
Math.pow(2,53)-1;//9007199254740991
```
正则修饰符：
```
i;//ignorCase 忽略大小写；
m;//mutiple 允许多行匹配；
g;//globle 进行全局匹配，指匹配到目标串的结尾；
```