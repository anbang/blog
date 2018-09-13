Express渲染HTML的，现在前后端分离的项目比较多，很少会用

# 一
```javascript
//加载模板处理模块
var swig = require('swig');

//定义当前应用所使用的模板引擎
//第一个参数：模板引擎的名称，同时也是模板文件的后缀，第二个参数表示用于解析处理模板内容的方法
app.engine('html', swig.renderFile);

//设置模板文件存放的目录，第一个参数必须是"views"，第二个参数是目录
app.set('views', './views');

//注册所使用的模板引擎，第一个参数必须是 "view engine"，第二个参数和 [app.engine]这个方法中定义的模板引擎的名称（第一个参数）是一致的
app.set('view engine', 'html');

//在开发过程中，需要取消模板缓存，这个看实际需求
swig.setDefaults({cache: false});

```

# 二

看express的默认；(使用应用程序生成器工具 `express` 快速创建应用程序框架)

```
npm install express-generator -g
express dirname
```

这样会生成一个默认的渲染模板；内容如下

```javascript
// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');
```
默认使用jade来渲染html的；

# 三，在express基础上自定义渲染模板

其实和第一是一致的

```javascript
//加载模板处理模块
var ejs = require('ejs');

//定义当前应用所使用的模板引擎
//第一个参数：模板引擎的名称，同时也是模板文件的后缀，第二个参数表示用于解析处理模板内容的方法
app.engine('html', ejs.__express);

//设置模板文件存放的目录，第一个参数必须是views，第二个参数是目录
app.set('views', path.join(__dirname, 'views'));

//注册所使用的模板引擎，第一个参数必须是 view engine，第二个参数和app.engine这个方法中定义的模板引擎的名称（第一个参数）是一致的
app.set('view engine', 'html');
```
