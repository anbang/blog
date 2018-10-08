判断IE的一些兼容性；

```javascript
var isIE=!!window.ActiveXObject;//判断是不是IE；
var isIE6=isIE&&window.XMLHttpRequest;//判断是不是IE6；
```