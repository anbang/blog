封在Tool.prototype上的；在tool里装了一个属性；
``` 
this.flag = “getElementsByClassName” in document;
```

这个在IE678是不兼容的；如果这个是true说明是标准浏览器；

如果是false，说明是IE大爷；

打印在控制台如下：

在chrome等标注浏览器下：

封装DOM库0

在低版本的IE大爷上；封装DOM库1

但是这个库封好后，我就后悔了！开始我是想模拟的跟原生方法一样；context.getElementclassName()这样的

但是实际使用中发现，这玩意纯粹是为了装逼！

在webstrom中是如下的；

封装DOM库

没有提示参数的功能了；我在封的时候特意把形参写在里面，如下风格的：

 
```
getElementsByClass: function () {
    var cName = arguments[0],
        context = arguments[1] || document,
        ary = [];
    if (this.flag) {
        return this.listToArray(context.getElementsByClassName(cName));
    }
    var allNode = context.getElementsByTagName("*"),
        reg = new RegExp("(?:^| +)" + cName + "(?: +|$)");
    for (var i = 0; i < allNode.length; i++) {
        var cur = allNode[i];
        if (reg.test(cur.className)) {
            ary.push(cur);
        }
    }
    return ary;
},
```
用的arguments[0]/[1]这种的来表示的；以后再升级下DOM，这个用起来不怎么舒服；

DOM库第二版的封装如下：

```

var Tool = function () {//构造函数模式；用的时候需要new一下；
    this.flag = "getElementsByClassName" in document;
    //getElementsByClassName 在IE678中是不存在的。用这个来判断是不是低版本的IE浏览器；
    //每次只需要判断this.flag是否存在就可以了；如果存在就是标准浏览器，如果不存在就是IE；
};
Tool.prototype = {//方法是定义在Tool的prototype上的；
    constructor: Tool,//重写prototype后，prototype的constructor已经不是原来的Tool了；需要手动给他强制写会到Tool上去；
    toJSON: function () {//将json字符串转化为json对象
        var jsonObj = null,
            jsonStr = arguments[0];
        try {
            jsonObj = JSON.parse(jsonStr);//如果支持 JSON.parse则直接使用 JSON.parse将JSON字符串转换为JSON对象。
        } catch (e) {
            jsonObj = eval("(" + jsonStr + ")");
        }
        return jsonObj;
    },
    isType: function () {//判断数据类型；
        var value = arguments[0],
            type = arguments[1] || "Object",
            reg = new RegExp("\\[object " + type + "\\]", "i");
        return reg.test({}.toString.call(value));//{}不可以是[]，因为[]就不是Object.prototype；
    },
    listToArray: function () {//类数组转化为数组；
        var ary = [],
            likeAry = arguments[0];
        if (this.flag) {
            ary = [].slice.call(likeAry, 0);
        } else {
            for (var i = 0; i < likeAry.length; i++) {
                ary.push(likeAry[i]);
            }
        }
        return ary;
    },
    getElementsByClass: function () {
        var cName = arguments[0],
            context = arguments[1] || document,
            ary = [];
        if (this.flag) {
            return this.listToArray(context.getElementsByClassName(cName));
        }
        var allNode = context.getElementsByTagName("*"),
            reg = new RegExp("(?:^| +)" + cName + "(?: +|$)");
        for (var i = 0; i < allNode.length; i++) {
            var cur = allNode[i];
            if (reg.test(cur.className)) {
                ary.push(cur);
            }
        }
        return ary;
    },
    getChildren: function () {//获取指定节点名的元素节点；第二个参数如果不穿，则返回obj下的所有子元素节点；
        var parent = arguments[0],
            tagName = arguments[1],
            allNode = parent.childNodes,
            ary = [],
            reg = new RegExp("^" + tagName + "$", "i");
        for (var i = 0; i < allNode.length; i++) {
            var cur = allNode[i];
            if (cur.nodeType === 1) {
                if (tagName) {
                    if (reg.test(cur.nodeName)) {
                        ary.push(cur);
                    }
                    continue;
                }
                ary.push(cur);
            }
        }
        return ary;
    },
    getFirst: function () {//第一个元素节点；
        var curEle = arguments[0],
            children = this.getChildren(curEle);
        return children.length > 0 ? children[0] : null;
    },
    getLast: function () {//最后一个元素节点；
        var curEle = arguments[0],
            children = this.getChildren(curEle);
        return children.length > 0 ? children[children.length - 1] : null;
    },
    getPre: function () {//上一个哥哥节点；
        var curEle = arguments[0];
        if (this.flag) {
            return curEle.previousElementSibling;
        }
        var pre = curEle.previousSibling;
        while (pre && pre.nodeType !== 1) {
            pre = pre.previousSibling;
        }
        return pre;
    },
    getPres: function () {//获得所有哥哥们；
        var curEle = arguments[0],
            ary = [],
            attr = this.flag ? "previousElementSibling" : "previousSibling";
        var pre = curEle[attr];
        while (pre) {
            if (pre.nodeType === 1) {
                ary.unshift(pre);
            }
            pre = pre[attr];
        }
        return ary;
    },
    getNext: function () {//下一个弟弟节点，第一个弟弟节点；
        var curEle = arguments[0];
        if (this.flag) {
            return curEle.nextElementSibling;
        }
        var next = curEle.nextSibling;
        while (next && next.nodeType !== 1) {
            next = next.nextSibling;
        }
        return next;
    },
    getNexts: function () {//获取所有的弟弟们；
        var curEle = arguments[0],
            ary = [],
            next = this.getNext(curEle);
        while (next) {
            ary.push(next);
            next = this.getNext(next);
        }
        return ary;
    },
    getSibling: function () {//获取上一个哥哥和下一个弟弟；
        var curEle = arguments[0],
            ary = [],
            pre = this.getPre(curEle),
            next = this.getNext(curEle);
        pre ? ary.push(pre) : void 0;
        next ? ary.push(next) : void 0;
        return ary;
    },
    getSiblings: function () {//获取所有的兄弟们（除了自己）
        var curEle = arguments[0],
            pres = this.getPres(curEle),
            nexts = this.getNexts(curEle);
        return pres.concat(nexts);
    },
    getIndex: function () {//获取元素的索引值；
        var curEle = arguments[0];
        return this.getPres(curEle).length;
    },
    getCss: function () {//获取CSS的属性值；att是attribute的缩写；
        var attr = arguments[0],
            curEle = arguments[1],
            reg = /^(?:margin|padding|border|float|position|display|background|backgroundColor)$/;
        var value = this.flag ? window.getComputedStyle(curEle, null)[attr] : curEle.currentStyle[attr];
        return !reg.test(attr) ? parseFloat(value) : value;
    },
    setCss: function () {//设置CSS属性值；
        var curEle = arguments[0],
            attr = arguments[1],
            value = arguments[2];
        switch (attr) {
            case "opacity":
                curEle["style"][attr] = value;
                curEle["style"]["filter"] = "alpha(opacity=" + (value * 100) + ")";
                break;
            case "zIndex":
                curEle["style"][attr] = value;
                break;
            default:
                curEle["style"][attr] = !isNaN(value) ? value += "px" : value;
        }
    },
    setGroupCss: function () {//给元素设置一组属性；
        var curEle = arguments[0],
            cssObj = arguments[1];
        for (var key in cssObj) {
            this.setCss(curEle, key, cssObj[key]);
        }
    },
    hasClass: function () {//判断是否有某个className；
        var cName = arguments[0],
            curEle = arguments[1],
            reg = new RegExp("(?:^| +)" + cName + "(?: +|$)");
        return reg.test(curEle.className);
    },
    addClass: function () {//增加类样式的方法；
        var cName = arguments[0], curEle = arguments[1];
        if (!this.hasClass(cName, curEle)) {
            curEle.className += " " + cName;
        }
    },
    removeClass: function () {//移除类样式的方法；
        var cName = arguments[0],
            curEle = arguments[1],
            reg = new RegExp("(?:^| +)" + cName + "(?: +|$)", "g");
        if (this.hasClass(cName, curEle)) {
            curEle.className = curEle.className.replace(reg, " ");
        }
    },
    offset: function () {//获取偏移量；
        var curEle = arguments[0],
            par = curEle.offsetParent,
            left = curEle.offsetLeft,
            top = curEle.offsetTop;
        while (par) {
            left += par.offsetLeft;
            top += par.offsetTop;
            if (navigator.userAgent.indexOf("MSIE 8.0") <= -1) {
                left += par.clientLeft;
                top += par.clientTop;
            }
            par = par.offsetParent;
        }
        return {left: left, top: top};
    },
    insertAfter: function () {//在目标元素oldEle后面插入新元素newEle；
        var newEle = arguments[0],
            oldEle = arguments[1],
            next = this.getNext(oldEle), par = oldEle.parentNode;
        next ? par.insertBefore(newEle, next) : par.appendChild(newEle);
    },
    prependChild: function () {//把一个元素节点添加为此元素的第一个子节点；
        var curEle = arguments[0],
            par = arguments[1],
            first = this.getFirst(par);
        first ? par.insertBefore(curEle, first) : par.appendChild(curEle);
    },
    html: function () {//获取元素的innerHTML；
        var curEle = arguments[0],
            str = arguments[1] || "";
        if (!str) {
            return curEle.innerHTML;
        }
        curEle.innerHTML = str;
    }
};
~function () {//在DOM库上增加方法，同时不影响原来的方法；在类的原型上增加方法；
    var strPro = String.prototype,
        aryPro = Array.prototype,
        objPro = Object.prototype;
    aryPro.myDistinct = function () {//数组去重；
        var obj = {};
        for (var i = 0; i < this.length; i++) {
            var cur = this[i];
            if (obj[cur] == cur) {
                this.splice(i, 1);
                i--;
                continue;
            }
            obj[cur] = cur;
        }
        obj = null;
        return this;
    };
    aryPro.myForEach = function () {//forEach兼容性处理；
        var fn = arguments[0],
            context = arguments[1] || window;
        if (this.forEach) {
            this.forEach(fn, context);
            return this;
        }
        for (var i = 0; i < this.length; i++) {
            fn.call(context, this[i], i, this);
        }
        return this;
    };
    objPro.myHasPubProperty = function () {//是否为公有属性；
        var attr = arguments[0];
        return (attr in this) && !this.hasOwnProperty(attr);
    };
    strPro.myTrim = function () {//去除首尾空格；
        return this.replace(/(^\s*|\s*$)/g, "");
    };
    strPro.mySub = function () {//是不是有效的
        var len = arguments[0] || 10,
            isD = arguments[1] || false,
            str = "",
            n = 0;
        for (var i = 0; i < this.length; i++) {
            var s = this.charAt(i);
            /[\u4e00-\u9fa5]/.test(s) ? n += 2 : n++;
            if (n > len) {
                isD ? str += "..." : void 0;
                break;
            }
            str += s;
        }
        return str;
    };
    strPro.myFormatTime = function () {//时间格式化；
        var reg = /^(\d{4})(?:-|\/|\.|:)(\d{1,2})(?:-|\/|\.|:)(\d{1,2})(?:\s+)(\d{1,2})(?:-|\/|\.|:)(\d{1,2})(?:-|\/|\.|:)(\d{1,2})$/g,
            ary = [];
        this.replace(reg, function () {
            ary = ([].slice.call(arguments)).slice(1, 7);
        });
        var format = arguments[0] || "{0}年{1}月{2}日 {3}:{4}:{5}";
        return format.replace(/{(\d+)}/g, function () {
            var val = ary[arguments[1]];
            return val.length === 1 ? "0" + val : val;
        });
    };
    strPro.myQueryURLParameter = function () {//是否是有效URL
        var reg = /([^?&=]+)=([^?&=]+)/g, obj = {};
        this.replace(reg, function () {
            obj[arguments[1]] = arguments[2];
        });
        return obj;
    }
}();
```

使用方法如下：算是API把；
```
1、如何调用
   tools.js 使用的是构造函数模式开发的类库，使用的时候需要创建一个实例，例如:
   var t=new Tool;
   t.getElementsByClass([className],[context]);
 
2、提供了那些的方法
   1)getElementsByClass
     作用：在指定的上下文中根据元素的类名(class样式名)获取对应的元素,旨在解决getElementsByClassName在IE6~8下不兼容
     参数：
       [className] 样式名称-->"string"
       [context] 上下文(非必填，默认是document)-->"object"
     返回值：我们获取的元素集合-->"Array"
   2)setGroupCss
     t.setGroupCss(oDiv,{
        left:100,
        top:20,
        opacity:0.3
     });
 
3、扩展(不修改原有的类但是可以使用原有类的方法)
  自定义类实现继承我们的Tool类，例如：
  1)原型链继承
  var Fn=function(){}
  Fn.prototype=new Tool;
  var f=new Fn;
  f就可以调用我们的Tool上面的方法了；因为f是Fn的一个实例了，而且f会通过；f.__proto__也就是Fn.prototype；找到Tools的函数；
  扩展的时候，也可以在f上直接f.XXX来扩展；
```
