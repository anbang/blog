vue2.x的vue cli 基于webpack的仓库；

构架压缩文件是通过

npm run build

这样生成的文件，默认是绝对路径的

比如下面

``` 
<!doctype html>
<html>
<head>
  <meta charset=UTF-8>
  <meta name=viewport content="width=device-width,user-scalable=no,initial-scale=1,maximum-scale=1,minimum-scale=1">
  <meta http-equiv=X-UA-Compatible content="ie=edge">
  <title>豆瓣(手机版)</title>
  <link href=/static/css/app.e2a2402eb9f343d1cc9259e3a3c4f601.css rel=stylesheet>
</head>
<body>
<div id=app></div>
<script type=text/javascript src=/static/js/manifest.7e1a3dc707e86aaef731.js></script>
<script type=text/javascript src=/static/js/vendor.518c168bf335d8505f46.js></script>
<script type=text/javascript src=/static/js/app.f968f598b314d2fdf4db.js></script>
</body>
</html>
```

里面别的包文件引入图片等也都是相对于跟路径的，放在服务器上配置下就可以的；

这样如果你本地访问是不行的；或者你是一个demo的演示，纯静态展示的（数据是通过jsonp抓取的）；

可以把文件全部生产相对的路径的；都是引入相对文件，这样静态也可以；

配置是修改 config/index.js 的文件

``` 
// see http://vuejs-templates.github.io/webpack for documentation.
var path = require('path')
 
module.exports = {
  build: {
    env: require('./prod.env'),
    index: path.resolve(__dirname, '../dist/index.html'),
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    productionSourceMap: true,
    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],
    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  },
  dev: {
    env: require('./dev.env'),
    port: 8080,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {},
    // CSS Sourcemaps off by default because relative paths are "buggy"
    // with this option, according to the CSS-Loader README
    // (https://github.com/webpack/css-loader#sourcemaps)
    // In our experience, they generally work as expected,
    // just be aware of this issue when enabling this option.
    cssSourceMap: false
  }
}
```

重点看

``` 
assetsPublicPath: '/',
```

这里是资源路径；把它改为相对的即可

``` 
assetsPublicPath: './',

```
这时候build出来的文件，就是纯静态的里。