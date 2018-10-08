JSON仅仅只是一种数据格式；

```
var obj1={};//JS对象；
var obj2={width:100,height:200};//JS对象；
var obj3={'width':100,'height':200};//JS对象
var obj4={"width":100,"height":200};//这个可以叫JSON格式的JavaScript对象；
var obj5='{"width":100,"height":200}';//JSON格式的JavaScript字符串；
```
JSON格式是为了跨平台交流数据用的；

JSON是严格模式的JavaScript对象表示法；JSON的属性名必须有双引号，值如果是字符串，也必须用双引号；

JSON是一种格式，没有储存任何数据的能力，仅仅是一种规范；

稍微复杂一点的JSON格式
```
 //复杂格式的JSON格式
var a=[
    {"width":100,"height":200,"name":"broszhu"},
    {"width":100,"height":200,"name":"bros"},
    {"width":100,"height":200,"name":"zhu"}
];
//这个是叫JSON格式的数组，是JSON的稍微复杂一点的形式；
var str2='['+
    '{"width":100,"height":200,"name":"broszhu"},'+
    '{"width":100,"height":200,"name":"bros"},'+
    '{"width":100,"height":200,"name":"zhu"}'+
']';
//这个叫稍复杂一点的JSON格式的字符串；
```

JSON格式的字符串转成JSON对象；


```
 var strJSON='{"width":100,"height":200,"name":"broszhu"}';
var obj=JSON.parse(strJSON);//把JSON字符串转成JS对象；eval不如parse正规；
//关于JSON和eval需要注意的是：在代码中使用eval是很危险的，特别是用它执行第三方的JSON数据（其中可能包含恶意代码）时，尽可能使用JSON.parse()方法解析字符串本身。该方法可以捕捉JSON中的语法错误，并允许你传入一个函数，用来过滤或转换解析结果。
console.log(obj);
//当控制台抛出unexpect token错误； 代表是口令错误，不按照格式写代码；
```
把JS对象转成JSON格式的对象
```
 //把JS对象转成JSON格式的对象；
var obj={width:100,'height':200,"name":"broszhu"};
function toJOSN(obj){//只适合简单的JSON；把对象转成JSON格式的对象；
    var a=[];
    for(var attr in obj){
        var val=obj[attr];
        if(typeof val=="string"){
            a.push('"'+attr+"\":\""+val+"\"")
        }
        if(typeof  val=="number"||val=="boolean"||val==="null"){
            a.push('"'+attr+"\":"+val)
        }
    }
    var str="{"+ a.join(",")+"}";
    return str;
}
testJSON=toJOSN(obj);
console.log(testJSON);
//{"width":100,"height":200,"name":"broszhu"}
```
上面是简单模式的，还有复杂模式的对象转成JSON格式对象；

比如下面这种格式的
```
var obj=[
    {width:100,'height':200,"name":"broszhu"},
    {width:100,'height':200,"name":"broszhu",ssss:"",ooxx:{width:100,'height':200,"name":"broszhu"}}
];
```

JSON格式的字符串转成JS对象；
```
 //把JS对象转成JSON格式的对象；
var str1= '{width:100,"height":200,"name":"broszhu"}';
//var obj1=eval(str1); 这种的会报错；
/*
 {name:100} 这种合法的。在脚本快里定义标签
 {name:100,age:18} 这种的是错误的；，违法的；所以上面会报错
* */
/*
下面是合法的对象写法；出现在等号右边，或者括号里，是合法的，都是作为值存在的；
 var obj={name:"name",age:18};
 ;({name:"name",age:18})
 下面是违法的；
 {name:100,age:18}
* */
/*下面这种的就不会报错；两面夹括号就可以了，就不会当做脚本快。强制以数据的形式*/
 var obj2=eval("("+str1+")");//这种的不会报错
```
用try catch来搞；
```
 //把JS对象转成JSON格式的对象；
function JSONParse(str){
/*        if(window.JSON){//如果支持则直接用parse就可以了；
     //不能用if(JSON)会报错，但是以属性的方式就可以了，所以要价window；或者可以用typeof JSON=="object";
     return JSON.parse(str);
     }else{//
     eval("("+str+")");
     }
     //上面的if如果window上恰好有定义JSON就有风险了。所以最好用try catch；*/
    try{
        return JSON.parse(str);
    }catch (e){
        return eval("("+str+")");//eval力如果传攻击性代码，也会直接执行了，所以这个也是有风险的；
    }
}
```