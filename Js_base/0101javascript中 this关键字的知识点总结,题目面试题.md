this是什么：

this就是content，就是上下文；
是指一个行为发生的主体
也就是谁发生的（或谁执行的）this就是谁（.前面是谁，this就是谁）
自运行函数，里面的this都是window
点前面是谁，this就是谁；
 


作用域和上下文区别：

在哪干这件事，谁干的这件事；分别是作用域和上下文；

this关键字是谁，和在哪儿定义没有关系；只和在哪儿执行有关系；定义的时候不知道this是谁，只有运行的时候才知道this是谁；(变量属于哪个作用域，由它在哪儿（作用域）定义有关系；)

作用域的思考题：
``` 
var i=9;
function fo(){
    var i=0;
    return function (n) {
        return n+i++;
    }
}
var f=fo();
var a=f(15);alert(a);
var b=fo()(15);
var c=fo()(20);
var d=f(20);alert(d);
//结果是分别弹出15；21；
```

带有执行函数的，考查this，考查闭包；考查预解释；综合自考题；非常经典；
```
 var number=2;
var obj={
    number:4,
    fn1:(function(){
        this.number*=2;
        number=number*2;
        var number=3;
        return function () {
            this.number*=2;//this的number
            number*=3;//上一级的number
            alert(number);
        }
    })()
};
var fn1=obj.fn1;
alert(number);//4；因为自执行函数里的this是window；number变成4了
fn1();//9;相当于运行里面return的函数；number=3*3，所以number是9；全局number是8；
obj.fn1();//27;刚才的number变成9了；number*=3，所以结果是27;obj才number变成8；
alert(window.number);//8
alert(obj.number);//8
```