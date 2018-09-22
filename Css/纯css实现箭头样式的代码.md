实现向上，向下，向左、向右的；

HTML代码如下

```html
<div id="triangle-facing-top"></div>
<div id="triangle-facing-right"></div>
<div id="triangle-facing-bootom"></div>
<div id="triangle-facing-left"></div>
```

CSS代码

```css
#triangle-facing-top {
    display: inline-block;  margin: 72px;
    width: 120px; height: 120px;
 
    border-top: 24px solid; border-right: 24px solid;
    transform: rotate(315deg);
}
 
#triangle-facing-right {
    display: inline-block;  margin: 72px;
    width: 120px; height: 120px;
 
    border-right: 24px solid; border-bottom: 24px solid;
    transform: rotate(-45deg);
}
 
#triangle-facing-bootom {
    display: inline-block;  margin: 72px;
    width: 120px; height: 120px;
 
    border-bottom: 24px solid; border-left: 24px solid;
    transform: rotate(315deg);
}
 
#triangle-facing-left {
    display: inline-block;
    margin: 72px;
    border-left: 24px solid; border-bottom: 24px solid;
    width: 120px; height: 120px;
    transform: rotate(45deg);
}
```