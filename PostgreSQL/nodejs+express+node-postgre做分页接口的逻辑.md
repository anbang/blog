postgresql的封装方法参见 [PG封装](./NodeJS基于pg封装的postgresql封装.md)

需要做一个分页的接口供前端使用,代码如下

```javascript
//获取账号的交易列表
var express = require("express");
var router = express.Router();

var pgclient = require('../database/PG');// 引用上述文件
pgclient.getConnection();

//写日志
let log4js = require('../database/log_config');
let logger = log4js.getLogger('read_database');//此处使用category的值

let responseData = null;
router.use(function (req, res, next) {
    responseData = {
        code: 200,
        success: true,
        message: "success"
    }
    next();
})

router.get("/get_account_list", function (req, res, next) {
    var queryAccount = req.query.account;// ?account=2
    var queryPage = req.query.page;// ?page=2
    var page, //当前页数
        pages, // 合计总页数
        count; //总条数

    var OFFSETVAL;//前面忽略的条数
    var LIMITVAL = 20;//每页显示条数
    if (typeof Number(queryPage) !== "number") {
        page = 1;
    } else {
        page = Number(queryPage) || 1;
    }
    //TODO 筛选To里 不是失败的
    // is_stable === true  and  is_fork === false and is_invalid === false and is_fail === false
    pgclient.query('Select COUNT(1) FROM transaction WHERE "from" = $1 OR "to"=$1', [queryAccount], (count) => {
        let typeCountVal = Object.prototype.toString.call(count);
        if (typeCountVal === '[object Error]') {
            responseData = {
                count: 0,
                tx_list: [],
                code: 500,
                success: false,
                message: "count account Error"
            }
            res.json(responseData);
        } else {
            count = Number(count[0].count);
            if (count === 0) {
                responseData = {
                    count: 0,
                    tx_list: [],
                    code: 404,
                    success: false,
                    message: "account no found"
                }
                res.json(responseData);
            } else {
                pages = Math.ceil(count / LIMITVAL);
                //paga 不大于 pages
                page = Math.min(pages, page);
                //page 不小于 1
                page = Math.max(page, 1);
                OFFSETVAL = (page - 1) * LIMITVAL;
                // *,balance/sum(balance) 
                pgclient.query('Select exec_timestamp,level,hash,"from","to",is_stable,is_fork,is_invalid,is_fail,amount,mci FROM transaction WHERE "from" = $1 OR "to"=$1 order by exec_timestamp desc, level desc,pkid desc LIMIT $2 OFFSET $3', [queryAccount, LIMITVAL, OFFSETVAL], (data) => {
                    let typeVal = Object.prototype.toString.call(data);
                    if (typeVal === '[object Error]') {
                        responseData = {
                            tx_list: [],
                            page: 1,
                            count: 0,
                            code: 500,
                            success: false,
                            message: "Select exec_timestamp,level,hash,from,to,is_stable,is_fork,is_invalid,is_fail,amount,mci FROM transaction Error"
                        }
                        res.json(responseData);
                    } else {
                        responseData = {
                            tx_list: data,
                            page: Number(queryPage),
                            count: Number(count),
                            code: 0,
                            success: true,
                            message: "success"
                        }
                        //是否从此帐号发出
                        responseData.tx_list.forEach(item => {
                            if (item.from == queryAccount) {
                                item.is_from_this_account = true;
                            } else {
                                item.is_from_this_account = false;
                            }
                            //是否转给自己
                            if (item.from == item.to) {
                                item.is_to_self = true;
                            } else {
                                item.is_to_self = false;
                            }
                        })
                        res.json(responseData);
                    }
                });
            }
        }
    });
})
```