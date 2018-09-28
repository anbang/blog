##### global是全局对象的；

global的根本作用是全局变量的宿主；
global的属性都可以直接全局使用；
```
/*全局变量的声明方法，
1，global的属性；
2，不加var直接写的变量*/
/*引用方法，
可以用console.log(global.a)；
2，console.log(a)*/
```

永远使用var来声明对象，避免全局污染；

在模块内部声明的变量属性，不属于全局变量；var声明的属于模块本身，不属于全局；
```javascript
console.log(global===global.global);//true,global上会有一个global；
//with 方法的使用,下面就可以输出obj的name和age；
var obj={name:"objname",age:8}
with (obj){
    console.log(name);
    console.log(age);
}
```

global的属性有哪些？
```
* exports 导出对象
* require 函数
* module 当前模块
```

```
* __filename 当前文件的绝对路径,带文件名的
* __dirname 当前文件的相对路径
* 他们两个都是方法的参数，不是全局对象；
```

global是下面的思路
```javascript
(function (exports, require, module, __filename, __dirname) {
    console.log(__filename);
    console.log(__dirname);
})*/
```

下面是一些console方法的用法
```javascript
/*console.log('hello');//调用的就是process.stdout.write
process.stdout.write('hello');*/
 
console.log('log');
console.error('error');//出错的话error
console.info('info');//调试信息info
console.warn('warn');
 
//可以输对象
console.log({name:'zfpx'});//调用下面的方法
console.log(JSON.stringify({name:'zfpx'}));
console.log('%j',{name:'zfpx'});
//可以输入数学表达式
console.log(1+1);
//可以输入逻辑表达式
console.log(1==1); 
```

//下面是统计时间的
```javascript
console.time('while循环统计时间');
var i=0;
while(i++<50000000){
}
console.timeEnd('while循环统计时间');

//查看被哪些模块调用
console.trace('trace');
 
//测试,assert如果是正确什么都不做，如果是false则红色报错
console.assert(1==1);
console.assert(1==2);
```