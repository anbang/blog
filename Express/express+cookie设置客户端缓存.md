### 第一步.API写字符串cookie；

核心代码

```
req.cookies.set(‘userInfo’,JSON.stringify(responseData.data));
```
### 第二步：入口文件中，解析请求的cookie

**如果本地有cookie，每次请求的时候，浏览器会带过去**

```
req.userInfo=JSON.parse(req.cookies.get(“userInfo”));
```

### 第三步：渲染；把读取到的 req.userInfo 配置给页面使用；

### 第四步：就是页面的模板引擎判断了；


## app.js 文件；

```javascript
var express = require('express');
var mongoose = require('mongoose');
var bodyParser = require('body-parser');
var Cookies = require('cookies')
var app = express();
swig.setDefaults({ cache: false });
//设置读取Body
app.use(bodyParser.urlencoded({ extended: true }));
app.use(function (req, res, next) {
  req.cookies=newCookies(req, res);
  req.userInfo= {};
  if (req.cookies.get('userInfo')) {
    try {
    req.userInfo=JSON.parse(req.cookies.get('userInfo'));
    } catch (e) { }
    console.log(req.userInfo._id)
  }
  next();
})
```

## api.js（接口文件）
```javascript
//是否在数据库中
User.findOne({
    'username':username,
    'password':password,
    }).then(function(data){
        if(!data){
            responseData.code=4;
            responseData.message='Error !!!!'
            returnres.json(responseData)
        }else{
            responseData.message='success'
            responseData.data={
            _id:data._id,
            username:data.username,
        }
        req.cookies.set('userInfo',JSON.stringify(responseData.data));
        returnres.json(responseData)
    }
})
```
## main.js(路由配置的渲染文件)

```javascript
var express =require('express');
var router=express.Router();
router.get('/',function(req,res,next){
    res.render('main/index',{
        userInfo:req.userInfo
    });
})
module.exports=router;
```






