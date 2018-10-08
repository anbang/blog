场景描述： 使用JavaScript改变输入元素的值，例如使用jquery的`.val()`，将不会触发change事件。

此时如果监听change事件，是不会触发的

解决办法；在jquery设置val的时候；手动触发change事件；

```javascript
this.element.val(start.format("YYYY-MM-DD HH:mm:ss"));
this.element.trigger('change');
```


这样设置以后了，当赋值结束以后，就会触发一次change事件；
