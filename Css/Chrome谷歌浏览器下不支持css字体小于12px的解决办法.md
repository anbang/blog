chrome默认浏览器，会把小于12px的字体，改为12px

可以用
```
transform: scale()
```

这个来写；通过

```
transform-origin
```

来控制缩放的基点

HTML结构

```html
<span class="wui-txt-muted wui-txt-xxs">
    <span class="wui-txt-xxs-item">解决chrome浏览器字体不低于12px的限制 用wui-txt-xxs的时候，创建一个子标签，类目是wui-txt-xxs-item，内容放在子标签内 </span>
</span>

```
CSS的写法

```css
.wui-txt-xxs {
    font-size: 10px;
    display: inline-block;
    border: 1px solid red;
}
.wui-txt-xxs .wui-txt-xxs-item{
    font-size: 12px;
    transform: scale(0.8333);/*缩放，解决chrome浏览器字体不低于12px的限制*/
    display: block;
    transform-origin:0 88% 0;/*改变缩放基点*/
}
```

第二个红框的后面空白，就是缩小引起的缺陷；这是适合一段文字独占一行；这样就可以认为的把瑕疵掩盖掉；