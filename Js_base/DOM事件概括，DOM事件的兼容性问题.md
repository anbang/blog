标准浏览器中；
``` 
oDiv.addEventListener();//标准浏览器中的绑定事件；
oDiv.removeEventListener();//标准浏览器中的移除绑定事件；
```
在IE678的非标准浏览器下；
```
oDiv.attachEvent();//绑定事件
oDiv.detachEvent();//移除事件
```
```
//    投机创建12个fn: fn1...fn12
for (var i = 1; i <= 12; i++) {
var str = "function fn" + i + "(){console.log(" + i + ");}";
eval(str);
        }
for (var i = 1; i <= 12; i++) {
oDiv.addEventListener("click", eval("fn" + i), false);
// oDiv.attachEvent("onclick", eval("fn" + i));//这个是IE的方法，
}
//点击后输出：1,2,3,4,5,6,7,8,9,10,11,12
```
具体应用：
```
<div id="div1">我是的div1</div>
<script type="text/javascript">
var oDiv = document.getElementById("div1");
function fn() {
console.log(this.innerHTML);
    }
oDiv.addEventListener("click", fn, false);//结果输出：我是的div1
    //    oDiv.attachEvent("onclick", fn);//IE的，两个方法互不兼容；
```
以下是DOM2级事件的兼容性问题；

1、给同一个元素的同一个事件类型绑定多个方法，在标准浏览器下的执行顺序是按照绑定的先后顺序执行的，在IE678下，顺序是错乱的（事件队列执行顺序呢的兼容问题；）

2、在标准浏览中，给元素绑定事件（不管的DOM0 还是DOM2），方法里面的this是当前被绑定的这个元素，但是IE678中，this指向的是window（DOM二级事件的this兼容问题）

3、在标准浏览器中，是不允许给同一个元素的同一个事件类型绑定同一个方法的，但是IE678中没有做这个处理判断，可以给同一个元素的同一个事件类型绑定多次同一个方法（DOM2级事件，方法重复绑定兼容问题）

谈谈对事件的理解，DOM事件的总结；

1、事件的定义：事件就是一个行为，DOM元素天生就有的行为，分为事件绑定和事件触发；即使没有事件绑定方法也可以触发，只不过是什么都没有做而已；

2、JS中事件分为DOM0，DOM2级事件；DOM0级事件是定义在元素的私有属性上的，而DOM2级事件，是定义在EventTarget这个类上的，所以DOM0级比DOM2级性更更好一些（原型链–>原型链的机制，->jQuery—>自己写的DOM库）

3、DOM0级我们常用的事件有哪些？
```
on[click、mouseover、mouseout、mouseenter、mouseleave、mousemove、mousedown、mouseup、mousewheel、keyup、keydown、focus、blur、change..]
```
4、DOM2中的好处有哪些？（3条）（具体参照：http://taobao.fm/archives/1022）

1）、提供了一些DOM0级中没有的事件（DOM开头的），例如：DOMcontentLoaded（jQuery中的$(document).ready()就是用这个事件实现的）

jQuery中的$(document).ready实现的原理；
1、采用DOM2级事件绑定；
2、在标准浏览器中的是DOMContentLoaded事件绑定的，在IE浏览器中用的是onreadystatechange事件绑定的，并且把事件绑定在document；
5、当我们触发事件行为的时候，执行绑定的那个方法，不管是0还是2级都存在事件对象（事件对象中常用的属性和兼容处理）

6、不仅存在事件对象，事件还存在传播的机制（捕获和冒泡），我们还可以应用事件传播的机制使用事件委托处理我们的 项目需求（事件委托的原理：给大元素绑定事件，判断里面的事件源，根据不同的事件源执行不同的操作）

7、用DOM2级事件的时候，要注意几个兼容的问题，绑定和移除绑定的方法是不一样的，剩下的就是上面总结的的那三条了；

8、如何处理7中的兼容；

9、在事件中我们可以用观察者模式制定自定义的事件（观察者模式的原理）