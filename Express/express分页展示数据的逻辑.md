**场景** ：`Express` `pgsql`

# PG数据库的封装

## 封装为模块方便使用

文件名为 `PG.js` , 代码如下  

```javascript
const { Client } = require('pg');
const config = require('./config');//config的内容比较隐私，单独放在一个文件中，ignore掉该文件；
/* config内容如下
let config = {
    host: '192.168.11.111',
    port: 5432,
    user: "canonchain"",
    password: "czr123",
    database: "canonchain_explorer",
    // 扩展属性
    max: 20, // 连接池最大连接数
    idleTimeoutMillis: 3000, // 连接最大空闲时间 3s
}
module.exports = config;
*/

//写日志
let log4js = require('./log_config');
let pglogger = log4js.getLogger('pg_sql');//此处使用category的值

let client = new Client(config);

let PG = function () {
    pglogger.info("准备数据库连接...");
};
PG.prototype.getConnection = function () {
    client.connect(function (err) {
        if (err) {
            return pglogger.error('数据库链接失败:', err);
        }
        client.query('SELECT NOW() AS "theTime"', function (err, result) {
            if (err) {
                return pglogger.error('error running query', err);
            }
            pglogger.info("数据库连接成功...");
        });
    });
};
PG.prototype.query = function (sqlStr, values, cb) {
    let typeVal = Object.prototype.toString.call(values);
    if (typeVal === "[object Function]") {
        //查
        pglogger.info(sqlStr);
        cb = values;
        client.query(sqlStr,function (err, result) {
            // pglogger.info(`结果,err ${err},result:${result}`);
            if (err) {
                cb(err);
            } else {
                if (result.rows !== undefined) {
                    cb(result.rows);
                } else {
                    cb();
                }
            }
        });
    } else {
        //插入
        // pglogger.info(`${sqlStr},${values}`);
        client.query(sqlStr,values, function (err, result) {
            if (err) {
                cb(err);
            } else {
                if (result.rows !== undefined) {
                    cb(result.rows);
                } else {
                    cb();
                }
            }
        });
    }
};
module.exports = new PG();
```

# Express写接口

逻辑如下 

```javascript
let express = require("express");
let router = express.Router();
 
let Czr = require('../czr');
let czr = new Czr();
 
let pgclient = require('../PG');// 引用上述文件
pgclient.getConnection();
 
let responseData = null;
router.use(function (req, res, next) {
    responseData = {
        code: 0,
        message: ""
    }
    next();
})
 
// http://localhost:3000/api/get_accounts
router.get("/get_accounts", function (req, res, next) {
    let queryPage = req.query.page;// ?page=2
    let page, //当前页数
        pages, // 合计总页数
        count; //总条数
 
    let OFFSETVAL;//前面忽略的条数
    let LIMITVAL = 2;//每页显示条数
    if (typeof Number(queryPage) !== "number") {
        page = 1;
    } else {
        page = Number(queryPage) || 1;
    }
    pgclient.query("Select COUNT(1) FROM accounts", (count) => {
        console.log("count>>", count[0].count);
        count = count[0].count;
        pages = Math.ceil(count / LIMITVAL);
        //paga 不大于 pages
        page = Math.min(pages, page);
        //page 不小于 1
        page = Math.max(page, 1);
        OFFSETVAL = (page - 1) * LIMITVAL;
        // *,balance/sum(balance) 
        pgclient.query("Select * FROM accounts ORDER BY balance DESC LIMIT " + LIMITVAL + " OFFSET " + OFFSETVAL, (data) => {
            //改造数据 排名 , 金额，占比
            let basePage = Number(queryPage) - 1; // 1 2
            let accounts = data;
            accounts.forEach((element, index) => {
                //占比 element.balance / 1618033988*1000000000000000000
                element.proportion = ((1000000000000000000 / (1618033988 * 1000000000000000000)) * 100).toFixed(10) + "%";
                //改成CZR单位，并保留6位精度
                let tempVal = czr.utils.fromWei(
                    element.balance,
                    "czr"
                );
                let reg = /(\d+(?:\.)?)(\d{0,4})/;
                let regAry = reg.exec(tempVal);
                let integer = regAry[1];
                let decimal = regAry[2];
                if (decimal) {
                    while (decimal.length < 6) {
                        decimal += "0";
                    }
                }
                element.balance = integer + decimal; //TODO Keep 6 decimal places
                element.rank = LIMITVAL * basePage + (index + 1);
            });
 
            responseData.account = accounts;
            responseData.page = Number(queryPage);
            responseData.count = Number(count);
            responseData.code = 0;
            responseData.message = "success";
            res.json(responseData);
        });
    });
})
module.exports = router;
```