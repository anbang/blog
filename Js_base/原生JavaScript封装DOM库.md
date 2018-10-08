封装这个DOM是为了做模块化开发准备的，模块化开发主要表现是，

http://www.163.com/

http://www.sina.com.cn/

这类新闻网站；里面用了大量的选项卡；


这个用JavaScript设计模式中的单例模式进行封装；

DOM代码如下；
```javascript

var DOM={};//分类的空间，最基本单例模式的方式
//添加方法只需要在单利模式上加东西就可以了；为了避免混乱，我没有写在括号内；
//解决getElementsByClassName兼容性问题的；开始封这个方法的时候，是想用在选项卡的模块化开发的，但是后来发现这个方法很多用处；
DOM.getElesByClass=function (strClass,context){//;第一个参数是calss的值，第二个参数是上下文；可以只穿一个参数；
    context=context||document;//如果只传一个参数，那么上下文是document；这里是逻辑或的方式来处理；
    if(context.getElementsByClassName){//如果是标准浏览器，那么直接用getElementsByClassName这个就可以了；就不费事了；
        return context.getElementsByClassName(strClass);
    }else{//如果是非标准浏览器，也就是IE大爷；使用以下的
        strClass=strClass.replace(/^ +| +$/g,"");//去除首位空格的意思；
        var aClass=strClass.split(/ +/);//通过空格把传进来的字符串分割；
        var eles=context.getElementsByTagName("*");//获取所有的标签；
        for(var i=0;i<aClass.length;i++){
            var str=aClass[i];
            var reg=new RegExp("(?:^| )"+str+"(?: |$)");//?:是只捕获，不匹配的意思；正则不难，但是难记，需要记牢；
            var a=[];//定义一个数组，用来装结果的
            for(var j=0;j<eles.length;j++){
                var ele=eles[j];
                if(reg.test(ele.className)){//如果符合字符串的要求；
                    a.push(ele);//添加到数组中；
                }
            }
            eles=a;//把a的引用地址赋值给eles；（引用数据类型的）
        }
        return eles;
    }
};
//给className上增加一个class字符串,主要用在选项卡之类的模块化开发上；
DOM.addClass=function(ele,strClass){
    var reg=new RegExp("(?:^| )"+strClass+"(?: |$)");//创建一个可以匹配传进来的字符串的正则；
    if(!reg.test(ele.className))//如果传进来的元素中没有匹配的正则字符串；
        ele.className+=" "+strClass;//则在上面class上增加这个字符串；
};
//给className上删除一个class字符串,主要用在选项卡之类的模块化开发上；
DOM.removeClass=function(ele,strClass){
    var reg=new RegExp("(?:^| )"+strClass+"(?: |$)","g");//创建一个可以匹配传进来的字符串的正则；只匹配不捕获；
    ele.className=ele.className.replace(reg," ");//把匹配到的正则字符串，用" "来替换；这里不能用""来替换；否则容易出BUG，亲测模块化开发选项卡的时候如果用""会出现一个LI部分出问题；
};
//获得索引值，用于选项卡模块化开发的时候，获取当前操作的li是第几个li；
DOM.getIndex=function(ele){
    var n=0;//假设是第0项；
    var prev=ele.previousSibling;//如果有上一个哥哥节点；如果有哥哥的元素节点，说明就不是第0个了；
    while(prev){//如果有上一个节点
        if(prev.nodeType===1){//并且上一个节点是一个元素节点；
            n++;//n自增；有几个元素节点的哥哥，就自增几次；
        }
        prev=prev.previousSibling;//再获取自增后prev的previousSibling
    }
    return n;
};
//获取元素的兄弟们元素；选项卡模块化开发时候，需要获得所有兄弟们，然后把所有兄弟们的样式去掉；
DOM.siblings=function(ele){
    var allEles=ele.parentNode.childNodes;//获取他的所有兄弟们；无论是什么节点，都获取过来；
    var a=[];//定义一个空数组，用来储存找到的兄弟们；
    for(var i=0;i<allEles.length;i++){
        var tempEle=allEles[i];
        if(tempEle.nodeType===1&&tempEle!=ele){//如果是一个非自身的元素节点；
            a.push(tempEle);//添加到数组里就可以了；
        }
    }
    return a;//返回兄弟们；
};
//获得ele相邻的第一个哥哥元素;（上一个哥哥）
DOM.prev=function(ele){
    if(ele.previousElementSibling){//如果浏览器支持这个属性，则直接返回
        return ele.previousElementSibling;
    }
    //如果不支持上面的代码，则会执行这儿来
    for(var p=ele.previousSibling;p;p=p.previousSibling){//for循环的高级写法；
        if(p.nodeType===1){//如果p是一个元素节点直接返回；
            return p;
        };
    }
    return null;//如果没有，就直接返回null；代表没有上一个哥哥元素；
};
//获得ele所有的哥哥,第一种写法；
DOM.prevSiblings1=function(ele){
    var a=[];//保存ele所有的哥哥用
    var p=ele.previousSibling;
    while(p){
        if(p.nodeType===1){
            a.push(p);
        }
        p=p.previousSibling;
    }
    return a;//返回出去，是一个数组。哪怕只有一个哥哥也是个数组，和byclassname类似的；
};
//获得ele所有的哥哥,第二种写法；
DOM.prevSiblings2=function(ele){
    var a=[];//保存ele所有的哥哥用
    var childNodes=ele.parentNode.childNodes;
    for(var i=0;i<childNodes.length;i++){
        var temp=childNodes[i];
        if(temp==ele)return a;//如果第i是ele本身，就把a数组返回出去；
        if(temp.nodeType===1){
            a.push(temp);
        }
    }
    return a;
};
//获得ele相邻的第一个弟弟元素
DOM.next=function(ele){
    if(ele.nextElementSibling){
        return ele.nextElementSibling;
    }
    var n=ele.nextSibling;
    while(n){
        if(n.nodeType===1){
            return n;
        }
        n=n.nextSibling;
    }
    return null;
};
//获得ele所有的弟弟(第一种写法)
DOM.nextSiblings1=function(ele){
    var a=[];
    var next=ele.nextSibling;
    while(next){
        if(next.nodeType===1){
            a.push(next);
        }
        next=next.nextSibling;
    }
    return a;
};
//获得ele所有的弟弟(第二种写法)
DOM.nextSiblings2=function(ele){
    var a=[];
    var childNodes=ele.parentNode.childNodes;
    var flag=false;//设一个标识变量，默认为false;一但遇到自己，变为true，就可以执行a.push
    //在变为true之前，都是ele的哥哥
    for(var i=0;i<childNodes.length;i++){
        var temp=childNodes[i];
        if(flag&&temp.nodeType===1){
            a.push(temp);
        }
        if(temp==ele)flag=true;//在遇到自己之前，都是在浪费性能
    }
    return a;
};
//获得ele所有的弟弟(第三种写法),从最后一个往前退,
DOM.nextSiblings3=function(ele){
    var a=[];
    var childNodes=ele.parentNode.childNodes;
    //第三种技巧：反向循环，性能更优
    var i=childNodes.length-1;
    while(i>=0){
        var temp=childNodes[i];
        if(temp==ele)return a;//如果if后边只有一句脚本，则可以省略花括号，并且还可以写成一行
        if(temp.nodeType===1){
            a.push(temp);
        }
        i--;
    }
    return a;
};
//获得指定标签名的子元素,第二个参数可选
DOM.children=function(parent,tagName){
    var childNodes=parent.childNodes;
    var a=[];
    if(tagName===undefined){//如果第二个参数没有传
        //上边也可以写成typeof tagName=="undefined"，是一个意思；
        for(var i=0;i<childNodes.length;i++){
            var child=childNodes[i];
            if(child.nodeType===1){
                a.push(child);
            }
        }
    }else if(typeof tagName =="string"){//如果第二个参数传了，并且是正确的形式
        for(var i=0;i<childNodes.length;i++){
            var child=childNodes[i];
            //child.nodeName,child.tagName都是大写
            tagName=tagName.toUpperCase();
            if(child.tagName==tagName){//同时满足这两个条件：既是元素节点，标签名又相等
                a.push(child);
            }
        }
    }else{
        throw new Error("第二个参数类型错误");
    }
    return a;
};
//第二种写法；
DOM.children2=function(parent,tagName){
    var childNodes=parent.childNodes;
    if(typeof tagName=="undefined"){
        var reg=/^[A-Z]\w*$/;//没有传第二个参数，则表示把任意子元素都取到。所以这是一个很宽泛的正则
    }else if(typeof tagName=="string"){
        var reg=new RegExp("^"+tagName.toUpperCase()+"$");
    }
    var a=[];
    for(var i=0;i<childNodes.length;i++){
        var child=childNodes[i];
        if(reg.test(child.nodeName)){
            a.push(child);
        }
    }
    return a;
};
```