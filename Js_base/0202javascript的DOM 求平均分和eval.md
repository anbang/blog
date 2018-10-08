评委打分求平均分的；

去掉一个最大值，去掉一个最小值；然后剩下的分数求平均值；


代码如下：
``` 
function average(){
    var a=arguments;
    [].sort.call(a,function(a,b){return a-b;});//排序
    [].pop.call(a);//删除尾部
    [].shift.call(a);//删除首位
    var sum=eval([].join.call(a,"+"));
    return sum/a.length;
}
var target=average(56,75,98,35,56,78);
console.log(target);
```

eval
```
{mame:"zhu",age:22}//单独出现会报错；Uncaught SyntaxError: Unexpected token 不可预知的口令,代码写的不按格式:
obj={mame:"zhu",age:22};//可以出现的
({mame:"zhu",age:22});//代表对象；可以出现的
'{"mame":"zhu","age":"22","height":"175cm"}'//JSON
var obj2=eval('{"mame":"zhu","age":"22","height":"175cm"}');//这么写会出错；
var obj3=eval('({"mame":"zhu","age":"22","height":"175cm"})');//加个括号就可以了；
```