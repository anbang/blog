方法1、是自己挤出来的，用2个DIV实现，以前一直这么搞的，CSS没人交流，所以一直按照最笨的来；

今天学到一个新的方法；瞬间高大上；


方法2：加入下面两行就可以了；需要固定宽度；
```
    display：table-cell；
    vertical-align:middle;
```
一起的代码如下：
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        #div1{
            background: #990000;
            width: 36px;
        }
        #div2{
            font-size: 18px;
            width: 18px;
            background: red;
            margin: 0 auto;
            padding: 10px 0;
        }
        #div3{
            width: 50px;
            font-size: 28px;
            background: orange;
            text-align: center;
            display：table-cell；
            vertical-align:middle;
            padding: 10px 0;
        }
    </style>
</head>
<body>
<!--两个DIV的方法-->
<div id="div1">
    <div id="div2">朱安邦朱安邦朱安邦</div>
</div>
<br/>
<!--一个DIV的方法-->
<div id="div3">叫我朱哥叫我朱哥叫我朱哥</div>
</body>
</html>

```
`vertical-align`这个属性只是针对于行内块标签起作用；

行内快就是，既有行内元素的特性也有块级元素的特性：可以一行显示多个，同时又具有宽高；

标签分三种，行内元素，行内块元素，块级元素 