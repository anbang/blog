前端开发时，请求后台接口经常需要跨域，vue-cli实现跨域请求只需要打开config/index.js，修改如下内容即可。

例如要请求的接口url为http://172.3.2.1:8000/look/1

```javascript
module.exports = {
    dev:{
        proxyTable:{
            '/api':{
                target: 'http://172.3.2.1:8000',
                changeOrigin: true,
                pathRewrite: {
                  '^/api': ''
                }
            }
        }
    }
}
```

如果接口为本地的 `express` `koa` 做的接口

```javascript
module.exports = {
  dev: {
    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    //利用proxyTable我们能够将外部的请求通过webpack转发给本地，也就能够将跨域请求变成同域请求了。
    proxyTable: {
      '/api':{
        target: 'http://localhost:3000/',
        changeOrigin: true,
        pathRewrite: {
          '^/api': '/api'
        }
      }
    },
    //...
  }
  //...
}
```

注：这只是开发时候使用的，构建后不这么搞；