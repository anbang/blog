无论什么语言下，做跨域的核心就是 `"Access-Control-Allow-Origin"` 设置为`"*"`

在`Express`下是这么干的

```javascript
var app = express();
app.all('/api', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});
```

这样就OK了