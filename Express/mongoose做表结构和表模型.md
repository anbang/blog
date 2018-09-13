mongoose的表结构和表模型

- 表结构（/schemas/contents.js）
- 表模型（/models/Content.js）

# 一、表结构

```javascript
var mongoose = require(‘mongoose’);
var Content = mongoose.Schema;
//表结构
var contentSchema = new Content({
    title:String,
    category: {
        type:mongoose.Schema.Types.ObjectId,
        ref:'Category'
    },
    description: {
        type:String,
        default:''
    },
    content: {
        type:String,
        default:''
    }
});
module.exports = contentSchema;
```
# 二、表模型

```javascript
var mongoose = require(‘mongoose’);
var contentSchema = require(‘../schemas/contents’);//先引用表结构
var Content = mongoose.model(‘Content’, contentSchema);//指定表模型
module.exports = Content;
```

# 三、使用

```javascript
var Content = require(“../models/Content”)
router.get(“/content”, function (req, res, next) {
    Content.find().populate(‘category’).then(function (contents) {
        res.render(‘admin/content_index’, {
            userInfo:req.userInfo,
            contents:contents
        });
    })
})
```