### 1、定义表结构（/schemas/user.js）
```javascript
    var mongoose = require('mongoose');
    var Schema = mongoose.Schema;
    //表结构
    var userSchema = new Schema({
        username:String,
        password:String,
        isAdmin:{
            type :Boolean,
            default:false
        }
    });
    module.exports = userSchema;
```

### 2、定义表模型（/models/User.js）
```javascript
    var mongoose = require('mongoose');
    var userSchema = require('../schemas/user');//先引用表结构
    var User = mongoose.model('User', userSchema);//指定表模型
    module.exports = User;
```


### 3、api接口如下
```javascript
    var express = require("express");
    var router = express.Router();
    var User = require("../models/User");
    var responseData = null;
    router.use(function (req, res, next) {
        responseData= {
            code:0,
            message:""
        }
        next();
    })
    // api/user/register?username=zhuanbang&password=12345678&repassword=12345678
    router.post('/user/register', function (req, res, next) {
        var username=req.body.username;
        var password=req.body.password;
        var repassword=req.body.repassword;
        console.log('req.body', req.body)
        if(!username){
            responseData.code=1;
            responseData.message="username is null"
            returnres.json(responseData)
        }
        if(!password){
            responseData.code=2;
            responseData.message="password is null"
            returnres.json(responseData)
        }
        if(password!==repassword){
            responseData.code=3;
            responseData.message="password is diff"
            returnres.json(responseData)
        }
        User.findOne({"username":username}).then(function(data){
            if(!data){
                //插入
                var user=newUser({
                    "username":username,
                    "password":password
                });
                user.save().then(function(userInfo){
                    returnuserInfo;
                }).then(function(userInfo){
                    if(userInfo){
                        responseData.code=0;
                        responseData.message="success"
                        req.cookies.set('userInfo',JSON.stringify({
                        "username":username,
                        "_id":userInfo._id
                        }));
                        returnres.json(responseData)
                    }
                })
            }else{
                responseData.code=4;
                responseData.message="is in db"
                returnres.json(responseData)
            }
        })
        // res.json({"data":req});
    })
    router.post("/user/login",function(req,res,next){
        var username=req.body.username;
        var password=req.body.password;
        if(!username){
            responseData.code=1;
            responseData.message="username is null"
            returnres.json(responseData)
        }
        if(!password){
            responseData.code=2;
            responseData.message="password is null"
            returnres.json(responseData)
        }
        //是否在数据库中
        User.findOne({
            "username":username,
            "password":password,
        }).then(function(data){
            if(!data){
                responseData.code=4;
                responseData.message="Error !!!!"
                returnres.json(responseData)
            }else{
                responseData.message="success"
                responseData.data={
                    _id:data._id,
                    username:data.username,
                }
                req.cookies.set('userInfo',JSON.stringify(responseData.data));
                returnres.json(responseData)
            }
        })
    })
    router.get("/user/logout",function(req,res,next){
        req.cookies.set('userInfo',");
        responseData.message="success"
        returnres.json(responseData)
    })
    module.exports = router;

```




