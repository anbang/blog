面向对象编程：Object为导向的编程;

单例模式：
```
/*单例模式的*/
var obj={
    name:"zhuanbang",
    age:"25",
    height:"175",
    weight:"65",
    gender:"男",
    CSS:function css(){},
    HTML:function html(){},
    JS:function js(){}
};
/*另外一种写法*/
var DOM={};
DOM.insertAfter=function(){};
DOM.perpend=function(){};
DOM.getIndex=function(){};
```
工厂模式：
```
 /*工厂模式,有单利模式演变的*/
var shirt={material:"虎皮",size:"xl",weight:"",height:"",width:""};
var shirt1={material:"虎皮",size:"xl",weight:"",height:"",width:""};
var shirt2={material:"虎皮",size:"xl",weight:"",height:"",width:""};
var shir3={material:"虎皮",size:"xl",weight:"",height:"",width:""};
var shirt4={material:"虎皮",size:"xl",weight:"",height:"",width:""};
/*上面是单利模式，下面是工厂模式*/
function shirtFactory(){
    var obj=new Object;//建立一个对象
    obj.material="";
    obj.weight="";
    obj.height="";
    obj.width="";
    obj.size="";
    return obj;//返回出去
}
//面向对象的第一个任务：解决数据生产的方式；
// 基于对象，可以描述比较复杂的数据;面向对象，不仅可以描述，而且还可以生产复杂数据；

```

构造函数模式：
```
 /*构造函数*/
function shirt(material){
    this.material=material;
    this.weight="";
    this.height="";
    this.width="";
    this.size="";
}
var shirt1=new shirt("虎皮");//用new关键字去执行一个函数，则这个函数就被当成一个类来使用的；这个函数叫构造函数模式；
//构造函数的作用是初始化；
// 构造函数起到实例识别的作用
```

构造函数和工厂模式的区别；

1、构造函数内不用new Objecr；

2、构造函数不用return；

3、里面的obj换成了this；

构造函数解决了实例识别的问题；

原型模式：
```
 function shirt(material){
    this.material=material;
    this.weight="";
    this.height="";
    this.width="";
    this.size="";
}
//1，创建shirt类的实例；
//2、把实例返回；
//3、在此实例为上下文运行shirt函数；
//shirt.prototype;//任何一个函数都有此属性。但他只有当成类来使用才有意义的；
//把所有共享的方法，都保存在此属性上的；
shirt.prototype.holdWarm=function(){
    console.log("保暖")
};
var shirt1=new shirt();
var shirt2=new shirt();
console.log(shirt1.holdWarm===shirt2.holdWarm)//true;
//原型模式，解决了类的公用问题；
 shirt1.a[0]=99;//如果私有属性没有a。会找到原型上；这样就修改了类的共有属性的a；
shirt1.a=99;//没有读的过程，直接写，会在shirt1的私有属性上写一个a；
```