## 最常用的部分

    .(一个点） 

可能通俗的理解为从属关系。比如：你.身体.胳膊.手.手指头。这里的.就表示了一系列的从属关系。


     ()括号： 

加在一个方法名后面表示让这个方法运行。括号本身也有运行的意思，比如：(4,3,5,7),这个就会返回一个7. 

    var: 

JS中定义变是的关键字，如果定义变量时不使var关键字，则此变量为全局变量。 

    window 

是指浏览器对象，是JS编程中的顶级作用域，JS中的一切方法和属性，都是这个对象的后代 

    document 

文档或文档对象，通俗的讲就是：凡是我们能看的见编码，就属于文档。是window的子对象



    document.getElementById() 

通过id来获取一个HTML元素，以便我们用JS来控制操作。比如：document.getElementById(‘div1′).style.color=’red’;获取id为div1的那个HTML元素使它的文字颜色为红（.style.color);如果网页中（应该叫文档中）没有id是div1的元素，则得到一个null（空） 

    document.getElementsByTagName() 

在整个网页内通过标签名（tagName)获得一组HTML元素的集合对象。比如：document.getElementsByTagName(‘div’);就是在整个网页（文档）范围内获取所有的div元素，如果网页内没有div元素，返回一个空集合对象。 

    document. 

getElementsByTagName(‘div’).item(3); 

    从得到的一组div中取得第四个div元素。（3是索引值，从0开始，所以索引值为3就是第四个元素了）
 
    document. getElementsByTagName(‘div’).length。获取到的这一组div的个数（length是长度、个数的意思） 

    body： 

指的是网页中body标记这个对象。要写成document.body才可使用。 

    tagName： 

标记名、标签名。象div,ul,li,p,a这些都是HTML元素的标记名。 

    className： 

类名。比如：<div class=’c1’></div> 这个c1就叫className，在js里，就通过这个属性来获的一个HTML元素的类名。

## 变量类型部分 
``` 
    null： 
空值,是一个空指针，一般表示一个变量定义了，但是没有值（值为空） 
    undefined： 
未定义的或定义了而未赋值的 
    true：逻辑值中的真 
    false：逻辑值中的假 
    boolean:布尔型（逻辑型） 
    NaN: 
不是一个数（not a number的缩写），它属于number型 
    number: 
数字，Number也是个方法（首字母大写），用来强制把一个值转换为数 
    string:字符串 
    function:方法，是定义方法的关键字 
    Array: 
数组，最常用的JS数据对象，可以存放多个值，一般用 new Array()或一对[]来定义。比如：var arr=new Array() (等同于var arr=[]) 
    typeof: 
用来计算变量类型的运算符，比如alert(typeof “abcd”)出输出string,表示这是个字符串类型、
```
 

## 基本语法部分 
```
    object: 
对象，通俗的讲把一个东东当成一个整体来看待，可称做一个对象 
    switch： 
JS的基础语法之一，和case配合使用，表示判断的那个条件（请详见示例的解释） 
    case： 
原意为情况、实例的意思，在JS中和switch配合使用完成一个判断。 
    break: 
中断当前语句体的执行，执行下一个语句体（见示例来理解和return的区别） 
    return:中断程序的执行并返回。 
    = ： 
一个等号是赋值运算，把右边的值赋给左边的变量，是从右向左计算。并且运算优先级最低。 
    == ： 
两个等号是比较运算，会得到一个真（ture）或假（false）的逻辑值 
    ?: 
三元运算符，相当于if else。基本格式为：条件表达式?表达式一:表达式二 问号（？）之前的表示判断的条件，如果这个条件为真，则执行问号后边的语句。如果判断条件不成立，则执行冒号后边的语句 
    while(bool)： 
循环语法之一，和之相对应的有do{ }while 
    控制CSS的方法 
currentStyle 当前的样式，ie专有用法 
getComputedStyle 获得计算出的样式，和样式有关的js方法，w3c标准用法 
    js获取css样式的通用方法： 
ele.currentStyle?ele.currentStyle[attr]:getComputedStyle(ele,false)[attr] 
（上面语句中的ele表示DOM对象，attr是css属性名。） 
ele.style.filter=”alpha(opacity=50)”：IE中把某HTML对象的不透明度设为50%（半透明）
```
 

## 数组及字符串常用方法及属性 
```
    slice(start,end): 
从已有的数组返回指定位置的数组（或解释为：从一个数组里截取另外一个数组）。带两个参数，第一个参数表示截取的起始点，第二个参数表示截取的终点。如果没有第二个参加，则表示终点到原数组的结尾；如果参数为负数，则表示倒着取。比如：var a=[1,2,3,4,5];alert(a.slice(0,3)); 输出1，2，3；alert(a.slice(3));输出4，5; alert(a.slice(1,-1));输出2，3，4；alert(a.slice(-3,-2));输出3 
    join(): 
把一个数组的所有元素转换为字符串，并把这些字符串按参数指定的字符连接起来。如果没有参数，则默认以逗号连接。比如：var a=[1,2,3];alert(a.join());输出字符串1,2,3。alert(a.join(‘-‘));则输出1-2-3。 
    push()方法： 
压栈，把一个元素增加到数组的最后。 
    pop()方法： 
出栈，把数组的最后一个元素删除。 
    unshift(): 
把一个对象添加到一个数组的第一项（对比push） 
    shift ():把数组中的第一项删除（对比pop方法） 
    reverse():用来颠倒数组的顺序（把原数组倒过来）。 
    sort();对数组进行排序，不带参数是按增序重排数组。 
    splice(index,count,[ele1],[…..],[eleN])： 
从这个数组的index位置起（包括此位置），一共删除count项内容。第二个参数（count）必须是数字，但可以是 “0”，表示不删除。如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素 
后面方括号里的参数表示是可选参数。表示要添加到数组的新元素。从 index 所指的下标处开始插入。 
和slice方法一样也是删除数组中的元素，但和slice不同的是：这个里面的参数是被删除的位置，而不是保留的位置。 
示例：var arr = new Array(6); 
arr[0] = “George”;arr[1] = “John”;arr[2] = “Thomas”;arr[3] = “James”;arr[4] = “Adrew”;arr[5] = “Martin”;document.write(arr + “<br />”); 
arr.splice(2,0,”zhufeng”,”peixun”);document.write(arr + “<br />”); 
输出：George,John,zhufeng,peixun,Thomas,James,Adrew,Martin 
    str.substr(start,[length])： 
字符串截取方法，从start开始，一共截取length个字符，如果没有第二个参数，则到字符串结尾 
    str.substring(start,[end]) ： 
字符串截取方法，从start开始，截取到end这个位置，如果没有第二个参数，则到字符串结尾 
    str.split(separator,[limit]): 
以参数separator以分隔符，把字符串分成数组。第二个参数是可选的，表示返回的数组的最大长度。第一个参数，可以是一个字符串，也可以是正则。和数组方法join 执行的操作是相反的操作。 
    indexOf(str):取得一个字str这个字符串在这个字符串中的位置 
    lastIndexOf(); 
str.replace(regexp/substr,replacement):把str这个字符串中的第一个参数部分的内容，用第二个参数的内容替换；第一个参数可以是一个正则，也可以是一个字符串。字符串替换方法，详见正则教材
```
 

## DOM方法 
```
    createElement: 
创建元素的方法，比如：var o=document.createElement(‘div’);创建一个DIV元素。 
    appendChild: 
追加子元素的方法，比如document.body.appentChild(o);(承上句) 
    createTextNode:创建文本节点 
    nodeType: 
节点类型，如果返回值为1，则说明此是元素节点。 
    nodeValue:文本字节的值。（元素节点没有此属性） 
    firstChild:第一个子节点。 
    lastChild:最后一个子节点 
    childNodes:所有的子节点的集合。 
    previousSibling:上一个兄弟节点。（sibling是兄弟的意思） 
    nextSibling:下一个兄弟节点。 
    parentNode:父节点 
    getAttribute(name):获取元素属性的值（attribute是属性的意思） 
    insertBefore(newNode,targetNode)： 
把newNode这个节点插入到targetNode的前面。 
    clone()：复制节点。 
    removeChild():删除子节点。 
    document.createDocumentFragment()：创建文档碎片。
```
 
## 时间和数学常用方法 
```
    Math.ceil(): 
把带小数的数往上取，比如1.1取成2，0.2取成1,-1.2取成-1（注意这个是负数）。 
    Math.floor()：常用的数学方法，把带小数的数往下取，和上一个正相反 
    Math.random():成生一个随机数，介于0到1之间的。 
    Math.round():四舍五入 
    Math.abs()：取绝对值
```

## 其它常用全局方法 
```
    parseInt(): 
截取字符串前边的数字，如果一个字符串前部不是数字，则返回NaN。比如parseInt(‘12px’),则window.setTimeout(fn,interval):定时器，在指定的时间（由第二个参数interval指定）之后运行某个方法（就是第一个参数fn）一次，然后停下来 
    window.clearTimeout(Tid)： 
根据定时器id，清除一个定时器 
    window.setInterval(fn,interval): 
也是定时器，在指定的时间（由第二个参数interval指定）之后运行某个方法（就是第一个参数fn）n次，不停下来。此方法有返回值，是一个数字，表示的是fn在定时器队列里的序号。
```

## 与盒子模型相关的属性与方法
```
    clientWidth: 
是对象可见的宽度，包括width和padding的宽，但不包括滚动条和边线。 
    offsetWidth： 
是对象的可见宽度，包括边框、滚动条、padding、内容的宽度，会随窗口的显示大小改变。（不包括margin） 
    offsetTop : 
相对于offsetParent（定位参照物）的上边的距离。（常用） 
    offsetLeft : 
相对于offsetParent（定位参照物）的左边的距离。（常用） 
    scrollLeft: 
设置或获取位于对象左边界和对象中目前可见内容的最左端之间的距离(width+padding为一体),其实就是用来移动左右滚动条。 
    scrollTop: 
设置或获取位于对象最顶端和对象中可见内容的最顶端之间的距离；(height+padding为一体)，其实就是用来移动上下滚 
    clientLeft: 获取对象的border宽度 
    clientTop：获取对象的border高度 
    offsetParent : 
    当前对象的发生偏移量的那个元素。offsetLeft的值，就是以此对象为参照物得出的 
    scrollWidth ： 
是对象的实际内容（比如文字占的实际宽）的宽，不包边线宽度，会随对象中内容的多少改变（内容多了可能会改变对象的实际宽度），但内容溢出之后，下内边距（就是padding）不会被计算。 
    document.documentElement.clientWidth||document.body.clientWidth:获取浏览器可见区域的宽。
```
 

## 事件相关的属性与方法
```
    event: 
事件，在IE浏览器中是个全局属性，表示浏览器的事件对象。 
    target: 
目标，指的是在标准浏览器中触发事件的那个源头，就是事件传翻的开始的那个元素。 
    srcElement:意义同上，不过是IE浏览器中用的。 
    setCapture(); 
IE专用方法，让一个HTML元素一直捕捉鼠标事件，使这个元素不会丢掉鼠标事件。当一个元素上调用这个方法时，所有后续的鼠标事件都被引导到这个元素上。 
    releaseCapture(): 
和上一个方法配合使用，当一个元素上调用这个方法时，它释放对鼠标事件的捕捉。IE专用。 
    ele.addEventListener(eventType,fnHandler,boolean)： 
DOM2级事件处理方法：给ele（ele是事先定义的变量名，下同）元素的eventTypt这个事件绑定fnHandler这个处理方法。（IE9以下浏览器不支持） 
    ele.removeEventListener(evntType,fnHandler,boolean)： 
DOM2级事件处理方法：给ele元素的eventTypt这个事件移除fnHandler这个处理方法。（IE9以下浏览器不支持） 
    oEle.attachEvent(onEventType,fnHandler)：同addEventListener 方法，IE专用 
    oEle.detachEvent(onEventType,fnHandler)：同removeEventListenter方法，IE专用 
    e.stopPropagation()：标准浏览器中的阻止事件传播的方法（IE9以下浏览不支持）。 
    e.cancelBubble=true：IE中取消事件冒泡的方法，IE专用。 
    preventDefault: 
标准浏览器中阻止HTML元素默认行为的的方法。 
    event.returnValue=false：IE浏览器中阻止元素默认行为的属性。
```

## 错误和异常处理单词
```
    Error：错误。Error在js中是一个类 
    Console:控制台 
    Throw：抛出。Throw new Error(message)，抛出一个自定义异常 
    Syntax error:语法上的明显错误 
    Parsing error：语法错误（翻译为“解析错误”更准确） 
    Invalid character：无效字符错误 
    Operation aborted：操作被终止 
    Cannot read property ‘style’ of null:这是JS中最常见的错误提示，直译为“不能读null这个对象的style属性”。如果我们写这样一句脚本： 
document.getElementById(‘div1’).style.backgroundColor=”red”;而网页中又没有一个id叫”div1”的元素，则就会提示这个错误，因为document.getElementById(‘div1’)返回的值是null。千万要注意，这句话不是说style是null，而是说style前边的对象是null。 
    Cannot read property ‘style’ of undefined 这个和上面的错误提示类似，直译为“不能读undefined的style这个属性”，千万要注意，这句话不是说style是undefined，而是说style前边的对象是undefined。详见第一天教材中的解释。 
    The system cannot locate the resource specified：系统无法找到指定资源。 
    Invalid character:无效字符错误，和火狐中的非法字符(illegal character)错误，和opera中的ReferenceError（引用错误）引起的原因一样，就是你在js代码中插入乱码。 
    Exception thrown and not caught:异常被抛出但未被捕获。 
    typeError:类型错误。 
    rangeError:超出范围的错误，数组越界错误。 
    try-catch-finally语句 
    Failed to load resource:载入资源失败（找不到指定的图片或其它资源时，chrom会报错） 
```