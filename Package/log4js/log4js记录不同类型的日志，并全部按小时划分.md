**官网**:[npm上的log4js地址](https://www.npmjs.com/package/log4js)

当前使用的`log4js`版本的 `3.0.5` , 1.X版本不能这么写

写配置 `log_config.js`

```javascript
let log4js = require('log4js');
let path = require('path');
let fs = require('fs');
 
let basePath = path.join(__dirname, '/logs/');
let defaultPath = path.join(basePath, '/default_database/');
let writeDbPath = path.join(basePath, '/write_database/');
let readDbPath = path.join(basePath, '/read_database/');
 
let confirmPath = function (pathStr) {
    console.log("pathStr",pathStr);
    if (!fs.existsSync(pathStr)) {
        fs.mkdirSync(pathStr);
        console.log('createPath: ' + pathStr);
    }
};
//创建log的根目录'logs'
if (basePath) {
    confirmPath(basePath);
    //根据不同的logType创建不同的文件目录
    confirmPath(defaultPath);
    confirmPath(writeDbPath);
    confirmPath(readDbPath);
}
 
log4js.configure({
    appenders: {
        out: {type: 'console'},
        default: {
            type: 'dateFile',
            filename: defaultPath,
            "pattern": "yyyy-MM-dd-hh.log",
            alwaysIncludePattern: true
        },
        write_db: {
            type: 'dateFile',
            filename: writeDbPath,
            "pattern": "yyyy-MM-dd-hh.log",
            alwaysIncludePattern: true
        },
        read_db: {
            type: 'dateFile',
            filename: readDbPath,
            "pattern": "yyyy-MM-dd-hh.log",
            alwaysIncludePattern: true
        }
    },
    categories: {
        default: {
            appenders: ['default'],
            level: 'info'
        },
        write_db: {
            appenders: ['write_db'],
            level: 'info'
        },
        read_db: {
            appenders: ['read_db'],
            level: 'info'
        }
    },
    replaceConsole: true              //是否替换console.log
});
 
 
module.exports = log4js;
```

写应用`index.js`，在这里演示怎么使用

```javascript
let log4js = require('./log_config');
 
let defaultLog = log4js.getLogger('default') ;        //此处使用category的值
let writeLog = log4js.getLogger('write_db') ;        //此处使用category的值
let readLog = log4js.getLogger('read_db')    ;      //此处使用category的值
 
defaultLog.info("default");
writeLog.info("000");
readLog.info("111");
```

这就OK了