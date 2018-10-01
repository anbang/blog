原生JS中，对事件对象(event)有2种主要的方法；
```
stopPropagation 和 preventDefault
```

第一个是禁止冒泡，第二个是阻止默认行为

注：这是原生JS的方法，并非jQuery的方法，event形参可以为任何变量，比如用e 这个也可以的；

```
    ele.onmouseover=function(event){
        event=event||window.event;
        if(event.stopPropagation){
            event.stopPropagation();//标准留言器中禁止冒泡；
            // preventDefault中文意思是阻止默认行为；
        }else{
            e.cancelBubble=true;//IE浏览器禁止冒泡；IE678
        }
    }
```

1、事件的禁止冒泡

```
    ele.onmouseover=function(event){
        event=event||window.event;
        if(event.stopPropagation){
            event.stopPropagation();//标准留言器中禁止冒泡；
            // preventDefault中文意思是阻止默认行为；
        }else{
            e.cancelBubble=true;//IE浏览器禁止冒泡；IE678
        }
    }
```

2、return的阻止

```
    ele.onmouseover=function(){
        return false
    }
```

区别。

return false 不仅阻止了事件往上冒泡，而且阻止了事件本身。

event.stopPropagation() 则只阻止事件往上冒泡，不阻止事件本身。

整理：

1.event.stopPropagation();

   事件处理过程中，阻止了事件冒泡，但不会阻击默认行为（可执行超链接的跳转）

2.return false;

   事件处理过程中，阻止了事件冒泡，也阻止了默认行为（不执行超链接的跳转）

还有一种有冒泡有关的：

```
event.preventDefault();
```

它的作用是：事件处理过程中，不阻击事件冒泡，但阻击默认行为（它只执行所有弹框，却没有执行超链接跳转）

~~