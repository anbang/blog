DOM主要是操作各个节点的；

分0级事件和2级时间；


 

可以用console.log(myTest)输出在控制台；然后看下；

DOM二级事件是定在在EventTarget上的；
``` 
 var oUl=document.getElementsByTagName("ul")[0],
    oLi=oUl.getElementsByTagName("li")[0],
    oComment=oUl.firstChild,
    oText=oLi.firstChild;
/*输出他们的原型，拉出原型链*/
console.dir(oUl);
//ul.__Proto__===>HTMLUListElement--->HTMLElement--->Element--->Node--->EventTarget--->Object
console.dir(oLi);
//li.__Proto__===>HTMLLIElement--->HTMLElement--->Element--->Node--->EventTarget--->Object
console.dir(oComment);
//#comment.__proto__===>Comment--->CharacterData--->Node--->EventTarget--->Object
//注意oComment前面有#号；
console.dir(oText);
//#text.__proto__===>Text--->CharacterData--->Node--->EventTarget--->Object
//DOM二级时间是定义在EventTarget上的；也就是【oUl.__proto__.__proto__.__proto__.__proto__.__proto__】Oul找五层；基类是Object；
```

HTML元素集合的特点：

1、活的，动态变化的；（根据网页上的DOM树的变化而变化）

2、重复活的的HTML集合是相同的，（相同DOM树上的地址）

3、集合里的元素顺序是依赖于文档上的元素顺序的。它自身不能改变的，只能随着DOM树的变化而变化；

排序的思路；用的是冒泡排序
```
 <body>
<ul><!--这是一个注释节点-->
    <li>12</li>
    <li>345</li>
    <li>35</li>
    <li>28</li>
    <li>23</li>
    <li>576</li>
</ul>
</body>
</html>
<script>
    var oLis=document.getElementsByTagName("li");
    var a=[];
    for(var i=0;i<oLis.length;i++){
        a.push(oLis[i]);
    }
    console.dir(a);
    a.sort(function(a,b){
        return a.innerHTML-b.innerHTML;//innerHtml是错的，必须要写出innerHTML
    });
    for(var i=0;i< a.length;i++){
        a[i].parentNode.appendChild(a[i])
    }
</script>
```
call和apply的区别“
```
 //call和apply本身也是方法；
function fn(){alert("执行fn")};
fn.call.call(fn);//相当于执行它fn
//fn.call.call(fn);里的call在调用call方法，执行前边call方法；并且让前边的call方法里的this指向fn；(指向第二个call的参数)
// 这个方法里的this是谁，就相当于谁在执行这个方法，就相当于fn.call();就相当于fn();
fn.call.call.call.call.call.call.call(fn);//相当于执行它fn
//当call没有传参的时候，里面的this是window；
fn.call();
fn.call(undefined);
fn.call(null,1,2,3,4,5,6,7);
//上面的this都是window；
fn.apply(null,[1,2,3,4,5,6,7])//apply是以数组的方式传入；
```

求数组的最大值，最小值；
```
 var b=[2,3,45,6,78,94,45];
console.log(Math.max.apply(null,b));//94
模拟slice

 //模拟slice
Array.prototype.slice2= function (a, b) {
    var ary=[];
    for(var i=a;i<b;i++){
        ary.push(this[i]);
    }
    return ary;
}
```

什么时候某一个实例能使用call来借用数组类上的方法呢？

凡是类数组的，都可以的借用call的；

这个方法里的逻辑操作这个实例

类数组就是类似数组的意思；(比如文档集合，arguments，jquery实例等都是类数组)

类数组转为数组的方法
```
 //；类数组可以用下面的方式转变类为数组；
var likeAry=[].slice.call(likeArray,0);
```

但是上面的方法在IE67里不支持的；需要解决下；解决方法可以用try catch；
```
 try{
    alert(broszhu);//出错，进入catch和finally；
}catch(e){
    console.log("catch来兜着")
}finally{
    console.log("出不出错我都要管")
}
```

类数组转化为数组:

```
 //类数组转化为数组；slice方法
function listToArray(eles) {
    var ary=[];
    try{
        ary=[].slice.call(eles,0);//IE7不支持
    }catch (e){
        for(var i= 0,len=eles.length;i<len;i++){
            ary.push(eles[i]);
        }
    }
    return ary;
};
```

改进一下
```
//类数组转化为数组；slice方法
function listToArray(eles) {
    if(!(eles.length&&eles[eles.length-1])){
        throw new Error("不能操作非法的类数组");
    }
    var ary=[];
    try{
        ary=[].slice.call(eles,0);//IE7不支持
    }catch (e){
        for(var i= 0,len=eles.length;i<len;i++){
            ary.push(this[i]);
        }
    }
    return ary;
};
```

类数组转化为数组；slice方法
```
function listToArray(eles) {
    if(!(typeof eles=="object"&&eles.length&&eles[eles.length-1])){//这样才严格
        throw new Error("不能操作非法的类数组");
    }
    var ary=[];
    try{
        ary=[].slice.call(eles,0);//IE7不支持
    }catch (e){
        for(var i= 0,len=eles.length;i<len;i++){
            ary.push(this[i]);
        }
    }
    return ary;
};
```