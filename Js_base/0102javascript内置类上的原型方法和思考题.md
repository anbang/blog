当声明定义一个数组的时候

var a=[1,2,3,4];

alert(a);//输出的是”[1,2,3,4]”;

之所以输出一串字符串，是因为隐式调用了toString的方法；toString是定义在原型上的方法；

原型上的方法，一般用this来操作；
```javascript
var a=[1,2,3,4];
alert(a);//会变成字符串；
console.log(a);//把a的结构输出在控制台；不会改变；不是字符串；
Array.prototype.toString=function(){
    return this.join("++")
};
alert(a);
console.log(a);
console.dir(a);
```

如果是基本数据，可以用alert，如果是检测对象函数之类的，用console.log；这个方法比alert更详细；

思考题：有一个数组，共m项，从这m项中，随机去除n项；n<m;

```
 function randomTest(min,max){
    return Math.floor(Math.random()*(max-min+1)+min);
}
var a=[];
for(var i=0;i<100;i++){
    a.push(i);//此时a是0到99的数,共100项目；
}
console.log(a);
var result=[];
for(var j=0;j<20;j++){
    var i=randomTest(0, a.length);
    result.push(i);
    a.splice(i,1);
}
console.log(result);//随机的20位数；
```
这个方法性能不好，删除后，索引位置会全部修改的；比较浪费性能；

可以用length的方法来优化性能；

```
 Array.prototype.randomTest2=function(n){
    //当前执行这个random方法的实例中，随机去掉n个数据，并且随机去掉的这些值保存在一个数据，返回；
    var result=[];
    for(var j=0;j<n;j++){
        //下面代码是取一个大于等于0，小于this.length的随机数；
        var random=Math.floor(Math.random()*(this.length));
        var val=this[random];
        result.push(val);
        this[random]=this[this.length-1];
        this.length=this.length-1;
    }
    return result;
};
var a=[41, 41, 19, 30, 79, 55, 67, 42, 15, 31, 44, 17, 4, 22, 53, 56, 25, 2, 76, 46];
var a2= a.randomTest2(8);
console.log(a2);//随机的20位数；
```
