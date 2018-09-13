文件结构如下

- app.js
- mongoose做表结构和表模型
- api.js
- main.js
- admin.js

# app.js 文件如下

```javascript
//加载express模块
var express = require('express');
//加载模板处理模块
var swig = require('swig');
//加载数据库模块
var mongoose = require('mongoose');
//加载body-parser，用来处理post提交过来的数据
var bodyParser = require('body-parser');
//加载cookies模块
var Cookies = require('cookies');
//创建app应用 => NodeJS Http.createServer();
var app = express();
 
var User = require('./models/User');
 
//设置静态文件托管
//当用户访问的url以/public开始，那么直接返回对应__dirname + '/public'下的文件
app.use( '/public', express.static( __dirname + '/public') );
 
//配置应用模板
//定义当前应用所使用的模板引擎
//第一个参数：模板引擎的名称，同时也是模板文件的后缀，第二个参数表示用于解析处理模板内容的方法
app.engine('html', swig.renderFile);
//设置模板文件存放的目录，第一个参数必须是views，第二个参数是目录
app.set('views', './views');
//注册所使用的模板引擎，第一个参数必须是 view engine，第二个参数和app.engine这个方法中定义的模板引擎的名称（第一个参数）是一致的
app.set('view engine', 'html');
//在开发过程中，需要取消模板缓存
swig.setDefaults({cache: false});
 
//bodyparser设置
app.use( bodyParser.urlencoded({extended: true}) );
 
//设置cookie
app.use( function(req, res, next) {
    req.cookies = new Cookies(req, res);
 
    //解析登录用户的cookie信息
    req.userInfo = {};
    if (req.cookies.get('userInfo')) {
        try {
            req.userInfo = JSON.parse(req.cookies.get('userInfo'));
 
            //获取当前登录用户的类型，是否是管理员
            User.findById(req.userInfo._id).then(function(userInfo) {
                req.userInfo.isAdmin = Boolean(userInfo.isAdmin);
                next();
            })
        }catch(e){
            next();
        }
 
    } else {
        next();
    }
} );
 
/*
* 根据不同的功能划分模块
* */
app.use('/admin', require('./routers/admin'));
app.use('/api', require('./routers/api'));
app.use('/', require('./routers/main'));
 
//监听http请求
mongoose.connect('mongodb://localhost:27018/blog', function(err) {
    if (err) {
        console.log('数据库连接失败');
    } else {
        console.log('数据库连接成功');
        app.listen(8081);
    }
});
```
# mongoose做表结构和表模型

参见文件：[mongoose做表结构和表模型](./mongoose做表结构和表模型)

文件如下

```
routers/admin.js
routers/api.js
routers/main.js
```

# api.js

```javascript
var express = require('express');
var router = express.Router();
var User = require('../models/User');
var Content = require('../models/Content');
 
//统一返回格式
var responseData;
 
router.use( function(req, res, next) {
    responseData = {
        code: 0,
        message: ''
    }
 
    next();
} );
 
/*
* 用户注册
*   注册逻辑
*
*   1.用户名不能为空
*   2.密码不能为空
*   3.两次输入密码必须一致
*
*   1.用户是否已经被注册了
*       数据库查询
*
* */
router.post('/user/register', function(req, res, next) {
 
    var username = req.body.username;
    var password = req.body.password;
    var repassword = req.body.repassword;
 
    //用户是否为空
    if ( username == '' ) {
        responseData.code = 1;
        responseData.message = '用户名不能为空';
        res.json(responseData);
        return;
    }
    //密码不能为空
    if (password == '') {
        responseData.code = 2;
        responseData.message = '密码不能为空';
        res.json(responseData);
        return;
    }
    //两次输入的密码必须一致
    if (password != repassword) {
        responseData.code = 3;
        responseData.message = '两次输入的密码不一致';
        res.json(responseData);
        return;
    }
 
    //用户名是否已经被注册了，如果数据库中已经存在和我们要注册的用户名同名的数据，表示该用户名已经被注册了
    User.findOne({
        username: username
    }).then(function( userInfo ) {
        if ( userInfo ) {
            //表示数据库中有该记录
            responseData.code = 4;
            responseData.message = '用户名已经被注册了';
            res.json(responseData);
            return;
        }
        //保存用户注册的信息到数据库中
        var user = new User({
            username: username,
            password: password
        });
        return user.save();
    }).then(function(newUserInfo) {
        responseData.message = '注册成功';
        res.json(responseData);
    });
});
 
/*
* 登录
* */
router.post('/user/login', function(req, res) {
    var username = req.body.username;
    var password = req.body.password;
 
    if ( username == '' || password == '' ) {
        responseData.code = 1;
        responseData.message = '用户名和密码不能为空';
        res.json(responseData);
        return;
    }
 
    //查询数据库中相同用户名和密码的记录是否存在，如果存在则登录成功
    User.findOne({
        username: username,
        password: password
    }).then(function(userInfo) {
        if (!userInfo) {
            responseData.code = 2;
            responseData.message = '用户名或密码错误';
            res.json(responseData);
            return;
        }
        //用户名和密码是正确的
        responseData.message = '登录成功';
        responseData.userInfo = {
            _id: userInfo._id,
            username: userInfo.username
        }
        req.cookies.set('userInfo', JSON.stringify({
            _id: userInfo._id,
            username: userInfo.username
        }));
        res.json(responseData);
        return;
    })
 
});
 
/*
* 退出
* */
router.get('/user/logout', function(req, res) {
    req.cookies.set('userInfo', null);
    res.json(responseData);
});
 
/*
* 获取指定文章的所有评论
* */
router.get('/comment', function(req, res) {
    var contentId = req.query.contentid || '';
 
    Content.findOne({
        _id: contentId
    }).then(function(content) {
        responseData.data = content.comments;
        res.json(responseData);
    })
});
 
/*
* 评论提交
* */
router.post('/comment/post', function(req, res) {
    //内容的id
    var contentId = req.body.contentid || '';
    var postData = {
        username: req.userInfo.username,
        postTime: new Date(),
        content: req.body.content
    };
 
    //查询当前这篇内容的信息
    Content.findOne({
        _id: contentId
    }).then(function(content) {
        content.comments.push(postData);
        return content.save();
    }).then(function(newContent) {
        responseData.message = '评论成功';
        responseData.data = newContent;
        res.json(responseData);
    });
});
 
module.exports = router;
```
# main.js

```javascript
let express     = require('express');
let router      = express.Router();
 
let Category    = require('../models/Category');
let Content     = require('../models/Content');
 
let data;
 
/*
* 处理通用的数据
* */
router.use(function (req, res, next) {
    data = {
        userInfo: req.userInfo,
        categories: []
    }
 
    Category.find().then(function(categories) {
        data.categories = categories;
        next();
    });
});
 
/*
* 首页
* */
router.get('/', function(req, res, next) {
 
    data.category = req.query.category || '';
    data.count = 0;
    data.page = Number(req.query.page || 1);
    data.limit = 10;
    data.pages = 0;
 
    let where = {};
    if (data.category) {
        where.category = data.category
    }
 
    Content.where(where).count().then(function(count) {
 
        data.count = count;
        //计算总页数
        data.pages = Math.ceil(data.count / data.limit);
        //取值不能超过pages
        data.page = Math.min( data.page, data.pages );
        //取值不能小于1
        data.page = Math.max( data.page, 1 );
 
        let skip = (data.page - 1) * data.limit;
 
        return Content.where(where).find().limit(data.limit).skip(skip).populate(['category', 'user']).sort({
            addTime: -1
        });
 
    }).then(function(contents) {
        data.contents = contents;
        res.render('main/index', data);
    })
});
 
router.get('/view', function (req, res){
 
    let contentId = req.query.contentid || '';
 
    Content.findOne({
        _id: contentId
    }).then(function (content) {
        data.content = content;
 
        content.views++;
        content.save();
 
        res.render('main/view', data);
    });
 
});
 
module.exports = router;
```

# admin.js

```javascript
var express = require('express');
var router = express.Router();
 
var User = require('../models/User');
var Category = require('../models/Category');
var Content = require('../models/Content');
 
router.use(function(req, res, next) {
    if (!req.userInfo.isAdmin) {
        //如果当前用户是非管理员
        res.send('对不起，只有管理员才可以进入后台管理');
        return;
    }
    next();
});
 
/**
 * 首页
 */
router.get('/', function(req, res, next) {
    res.render('admin/index', {
        userInfo: req.userInfo
    });
});
 
/*
* 用户管理
* */
router.get('/user', function(req, res) {
 
    /*
    * 从数据库中读取所有的用户数据
    *
    * limit(Number) : 限制获取的数据条数
    *
    * skip(2) : 忽略数据的条数
    *
    * 每页显示2条
    * 1 : 1-2 skip:0 -> (当前页-1) * limit
    * 2 : 3-4 skip:2
    * */
 
    var page = Number(req.query.page || 1);
    var limit = 10;
    var pages = 0;
 
    User.count().then(function(count) {
 
        //计算总页数
        pages = Math.ceil(count / limit);
        //取值不能超过pages
        page = Math.min( page, pages );
        //取值不能小于1
        page = Math.max( page, 1 );
 
        var skip = (page - 1) * limit;
 
        User.find().limit(limit).skip(skip).then(function(users) {
            res.render('admin/user_index', {
                userInfo: req.userInfo,
                users: users,
 
                count: count,
                pages: pages,
                limit: limit,
                page: page
            });
        });
 
    });
 
});
 
/*
* 分类首页
* */
router.get('/category', function(req, res) {
 
    var page = Number(req.query.page || 1);
    var limit = 10;
    var pages = 0;
 
    Category.count().then(function(count) {
 
        //计算总页数
        pages = Math.ceil(count / limit);
        //取值不能超过pages
        page = Math.min( page, pages );
        //取值不能小于1
        page = Math.max( page, 1 );
 
        var skip = (page - 1) * limit;
 
        /*
        * 1: 升序
        * -1: 降序
        * */
        Category.find().sort({_id: -1}).limit(limit).skip(skip).then(function(categories) {
            res.render('admin/category_index', {
                userInfo: req.userInfo,
                categories: categories,
 
                count: count,
                pages: pages,
                limit: limit,
                page: page
            });
        });
 
    });
 
});
 
/*
* 分类的添加
* */
router.get('/category/add', function(req, res) {
    res.render('admin/category_add', {
        userInfo: req.userInfo
    });
});
 
/*
* 分类的保存
* */
router.post('/category/add', function(req, res) {
 
    var name = req.body.name || '';
 
    if (name == '') {
        res.render('admin/error', {
            userInfo: req.userInfo,
            message: '名称不能为空'
        });
        return;
    }
 
    //数据库中是否已经存在同名分类名称
    Category.findOne({
        name: name
    }).then(function(rs) {
        if (rs) {
            //数据库中已经存在该分类了
            res.render('admin/error', {
                userInfo: req.userInfo,
                message: '分类已经存在了'
            })
            return Promise.reject();
        } else {
            //数据库中不存在该分类，可以保存
            return new Category({
                name: name
            }).save();
        }
    }).then(function(newCategory) {
        res.render('admin/success', {
            userInfo: req.userInfo,
            message: '分类保存成功',
            url: '/admin/category'
        });
    })
 
});
 
/*
* 分类修改
* */
router.get('/category/edit', function(req, res) {
 
    //获取要修改的分类的信息，并且用表单的形式展现出来
    var id = req.query.id || '';
 
    //获取要修改的分类信息
    Category.findOne({
        _id: id
    }).then(function(category) {
        if (!category) {
            res.render('admin/error', {
                userInfo: req.userInfo,
                message: '分类信息不存在'
            });
        } else {
            res.render('admin/category_edit', {
                userInfo: req.userInfo,
                category: category
            });
        }
    })
 
});
 
/*
* 分类的修改保存
* */
router.post('/category/edit', function(req, res) {
 
    //获取要修改的分类的信息，并且用表单的形式展现出来
    var id = req.query.id || '';
    //获取post提交过来的名称
    var name = req.body.name || '';
 
    //获取要修改的分类信息
    Category.findOne({
        _id: id
    }).then(function(category) {
        if (!category) {
            res.render('admin/error', {
                userInfo: req.userInfo,
                message: '分类信息不存在'
            });
            return Promise.reject();
        } else {
            //当用户没有做任何的修改提交的时候
            if (name == category.name) {
                res.render('admin/success', {
                    userInfo: req.userInfo,
                    message: '修改成功',
                    url: '/admin/category'
                });
                return Promise.reject();
            } else {
                //要修改的分类名称是否已经在数据库中存在
                return Category.findOne({
                    _id: {$ne: id},
                    name: name
                });
            }
        }
    }).then(function(sameCategory) {
        if (sameCategory) {
            res.render('admin/error', {
                userInfo: req.userInfo,
                message: '数据库中已经存在同名分类'
            });
            return Promise.reject();
        } else {
            return Category.update({
                _id: id
            }, {
                name: name
            });
        }
    }).then(function() {
        res.render('admin/success', {
            userInfo: req.userInfo,
            message: '修改成功',
            url: '/admin/category'
        });
    })
 
});
 
/*
* 分类删除
* */
router.get('/category/delete', function(req, res) {
 
    //获取要删除的分类的id
    var id = req.query.id || '';
 
    Category.remove({
        _id: id
    }).then(function() {
        res.render('admin/success', {
            userInfo: req.userInfo,
            message: '删除成功',
            url: '/admin/category'
        });
    });
 
});
 
/*
* 内容首页
* */
router.get('/content', function(req, res) {
 
    var page = Number(req.query.page || 1);
    var limit = 10;
    var pages = 0;
 
    Content.count().then(function(count) {
 
        //计算总页数
        pages = Math.ceil(count / limit);
        //取值不能超过pages
        page = Math.min( page, pages );
        //取值不能小于1
        page = Math.max( page, 1 );
 
        var skip = (page - 1) * limit;
 
        Content.find().limit(limit).skip(skip).populate(['category', 'user']).sort({
            addTime: -1
        }).then(function(contents) {
            res.render('admin/content_index', {
                userInfo: req.userInfo,
                contents: contents,
 
                count: count,
                pages: pages,
                limit: limit,
                page: page
            });
        });
 
    });
 
});
 
/*
 * 内容添加页面
 * */
router.get('/content/add', function(req, res) {
 
    Category.find().sort({_id: -1}).then(function(categories) {
        res.render('admin/content_add', {
            userInfo: req.userInfo,
            categories: categories
        })
    });
 
});
 
/*
* 内容保存
* */
router.post('/content/add', function(req, res) {
 
    //console.log(req.body)
 
    if ( req.body.category == '' ) {
        res.render('admin/error', {
            userInfo: req.userInfo,
            message: '内容分类不能为空'
        })
        return;
    }
 
    if ( req.body.title == '' ) {
        res.render('admin/error', {
            userInfo: req.userInfo,
            message: '内容标题不能为空'
        })
        return;
    }
 
    //保存数据到数据库
    new Content({
        category: req.body.category,
        title: req.body.title,
        user: req.userInfo._id.toString(),
        description: req.body.description,
        content: req.body.content
    }).save().then(function(rs) {
        res.render('admin/success', {
            userInfo: req.userInfo,
            message: '内容保存成功',
            url: '/admin/content'
        })
    });
 
});
 
/*
* 修改内容
* */
router.get('/content/edit', function(req, res) {
 
    var id = req.query.id || '';
 
    var categories = [];
 
    Category.find().sort({_id: 1}).then(function(rs) {
 
        categories = rs;
 
        return Content.findOne({
            _id: id
        }).populate('category');
    }).then(function(content) {
 
        if (!content) {
            res.render('admin/error', {
                userInfo: req.userInfo,
                message: '指定内容不存在'
            });
            return Promise.reject();
        } else {
            res.render('admin/content_edit', {
                userInfo: req.userInfo,
                categories: categories,
                content: content
            })
        }
    });
 
});
 
/*
 * 保存修改内容
 * */
router.post('/content/edit', function(req, res) {
    var id = req.query.id || '';
 
    if ( req.body.category == '' ) {
        res.render('admin/error', {
            userInfo: req.userInfo,
            message: '内容分类不能为空'
        })
        return;
    }
 
    if ( req.body.title == '' ) {
        res.render('admin/error', {
            userInfo: req.userInfo,
            message: '内容标题不能为空'
        })
        return;
    }
 
    Content.update({
        _id: id
    }, {
        category: req.body.category,
        title: req.body.title,
        description: req.body.description,
        content: req.body.content
    }).then(function() {
        res.render('admin/success', {
            userInfo: req.userInfo,
            message: '内容保存成功',
            url: '/admin/content/edit?id=' + id
        })
    });
 
});
 
/*
* 内容删除
* */
router.get('/content/delete', function(req, res) {
    var id = req.query.id || '';
 
    Content.remove({
        _id: id
    }).then(function() {
        res.render('admin/success', {
            userInfo: req.userInfo,
            message: '删除成功',
            url: '/admin/content'
        });
    });
});
 
module.exports = router;
```