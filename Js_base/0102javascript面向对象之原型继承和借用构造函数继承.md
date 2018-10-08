原型模式，解决了复用问题；

原型继承：一般原型会借用构造函数继承；

``` 
 function Person(name){
    this.weight=7;
    this.height=40;
    this.skin="yellow";
    this.gender="random随机";
    this.name=name;
    this.age=0;
};
Person.prototype.eat=function(){console.log("天生就会吃")};
Person.prototype.cry=function(){console.log("天生就会哭")};
function FE(){
};
FE.prototype=new  Person();
FE.prototype.writeCSS= function () {
    console.log("我会写CSS了")
};
FE.prototype.writeJS= function () {
    console.log("我会写JS了")
};
var tom=new FE;
tom.writeCSS();
var rose=new FE;
var jack=new FE;
jack.eat();//继承了
```
JS继承是基于原型链的；


JS中，prototype的作用是什么？

每一个函数上都有一个prototype属性，当函数作为一个类用的时候，才有意义；它保存了所有这个类的共享方法；这些方法解决了每个实例相同方法的复用问题；

JS中prototype属性和__proto__属性有什么不同？

prototype是函数上的，__proto__是实例上，但是他们指向同一个内存地址；

什么叫原型链

方法实例上方法运行时的查找顺序；

JS中的继承是怎么实现的；
```
/*原型继承*/
FE.prototype=new Person;//第一种继承
FE.prototype.__proto__=Person.prototype;//第二种继承,思考这种继承行不行？
FE.prototype=Person.prototype;//这种的不是继承。因为他们公用一个了； 这个是错误的
```

构造函数继承：使用call，让别的函数里面的this关键字，指向新函数中的this；

cll是function的方法；由一个function来执行，作用是使这个function执行并且让这个function里的this执行call里面的第一个参数；call方法后面的参数是传给function的；
```
//借用构造函数模式,使用call
function Person(name){
    this.weight=7;
    this.height=40;
    this.skin="yellow";
    this.gender="random随机";
    this.name=name;
    this.age=0;
};
Person.prototype.eat=function(){console.log("天生就会吃")};
Person.prototype.cry=function(){console.log("天生就会哭")};
function FE(name,age,degree){
    //Person(name);//当前FE这个函数的this是FE的实例；
    Person.call(this,name);
    //call,apply这两个方法是定义在FUnction.prototype上的；
    this.age=age;
    this.degree=degree;
};
FE1=new FE("broszhu",25,"dazhuan");
console.log(FE1);
```

