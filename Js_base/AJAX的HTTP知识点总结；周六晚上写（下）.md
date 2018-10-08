封装AJAX库和，DEMO展示；


封装的思路如下：
``` 
ajax({
    url:'',
    method:'',
    data:''
}).success(function(){
}).error(function(){
})
```
封装的代码如下：
```
(function () {
    //给window声明一个aa变量，类型为一个空对象
    this.aa = {};
    //给aa添加别名
    var xx = this.aa;
    //得到当前浏览器最合适的ajax对象，利用的是惰性函数
    function getXHR() {
        for (var xhrlist = [function () {
            return new XMLHttpRequest();
        }, function () {
            return new ActiveXObject('Microsoft.XMLHTTP');
        }, function () {
            return new ActiveXObject('Msxml2.XMLHTTP');
        }, function () {
            return new ActiveXObject('Msxml3.XMLHTTP');
        }], i = 0, obj = null; i < xhrlist.length; i++) {
            try {
                obj = xhrlist[i]();
                break;
            } catch (ex) {
            }
        }
        if (!obj) {
            throw new ReferenceError('the browser was not supported this feature');
        }
        getXHR = xhrlist[i];
        return obj;
    }
    //利用闭包，生成一个判断类型的函数
    var isType = function (type) {
        return function (data) {
            return Object.prototype.toString.call(data) == '[object ' + type + ']';
        }
    };
    //判断是不是对象类型
    var isObject = isType('Object');
    xx.ajax = function (obj) {
        //先判断传入的参数是不是一个对象，否则主动抛出错误
        if (!isObject(obj)) {
            throw new Error('wrong parameters')
        }
        //默认参数列表
        var _default = {
            url: '/',//请求的路径
            type: 'get',//http方法
            data: {},//发送到服务器的数据
            async: true,//是否为异步
            header: {},//自定义头信息
            dataType: 'text',//服务器放回的数据类型
            cache: false,//是否缓存
            timeout: Infinity//超时毫秒数
        }, temp = null;//声明一个后面要用的临时变量
        //把用户输入的对象值遍历到默认参数列表上
        for (temp in _default) {
            if (_default.hasOwnProperty(temp))
                _default[temp] = obj[temp];
        }
        //获取ajax对象
        var xhr = getXHR();
        //判断是否缓存，如果不缓存的话再判断有没有问号；如果没有问号就拼接一个问号加一个随机数，没有的话就直接拼接一个随机数
        if (!_default.cache) {
            var str = "";
            if (/\?/.test(_default.url)) {
                str = "&_=";
            } else {
                str = "?_=";
            }
            _default.url += str + ((Math.random() * 0xffffff) | 0)
        }
        //(!_default.cache)&&(_default.url+=(/\?/.test(_default.url)?"&_=":"?_=")+(Math.random()*0xffffff)|0);
        //判断发送的数据是一个对象的话，格式化为一个URI格式的字符串
        if (isObject(_default.data)) {
            var part = [];
            for (temp in _default.data) {
                if (_default.data.hasOwnProperty(temp)) {
                    part.push(encodeURIComponent(temp) + "=" + encodeURIComponent(_default.data[temp]));
                }
            }
            _default.data = part.join('&');
        }
        //判断输入的http方法是否合法
        if (/^(get|post|put|delete|head)$/igm.test(_default.type)) {
            //是否为get系的http方法
            if (!/^p/img.test(_default.type)) {
                //判断有没有问号
                if (/\?/.test(_default.url)) {
                    temp = '&';
                } else {
                    temp = "?";
                }
                _default.url += temp + _default.data;
                //把data清空
                _default.data = void 0;
            }
        } else {
            throw new Error('wrong http method');
        }
        //调用ajax对象的open方法，发送起始行数据包
        xhr.open(_default.type, _default.url, _default.async);
        //判断自定义头信息是否为对象
        if (isObject(_default.header)) {
            //遍历自定义头信息，然后调用setRequestHeader方法，添加到http request 首部里
            for (temp in _default.header) {
                if (_default.header.hasOwnProperty(temp)) {
                    xhr.setRequestHeader(temp, _default.header[temp]);
                }
            }
        }
        //判断服务器返回类型是否合法，不合法就直接抛出错误
        if (!/^(text|json)$/igm.test(_default.dataType)) {
            throw new Error('data type error');
        }
        //实例化一个promise对象，用于链式调用
        var _promise = new promise();
        //给ajax对象注册一个onreadystatechange的事件，用于处理服务器返回的数据
        xhr.onreadystatechange = function () {
            //判断http事务是否完成
            if (xhr.readyState == 4) {
                //判断http状态码是否为2开头的
                if (/^2\d{2}$/.test(xhr.status)) {
                    //把服务器返回的数据赋值给临时变量
                    temp = xhr.responseText;
                    //判断规定的数据类型是否为json
                    if (_default.dataType == 'json') {
                        try {
                            //将数据格式化为json格式，出现异常则执行promise的onerror事件
                            temp = JSONParse(temp)
                        } catch (e) {
                            return _promise.onerror(e, temp);
                        }
                    }
                    //直接将服务器返回的数据抛到promise对象的onsuccess方法里
                    _promise.onsuccess(temp);
                    //判断http状态码是4或5开头，就执行promise对象onerror的事件
                } else if (/^(4|5)\d{2}$/.test(xhr.status)) {
                    _promise.onerror(xhr.status);
                }
            }
        };
        //把格式化后的数据发送到服务器
        xhr.send(_default.data);
        //判断超时毫秒数是否合法
        if (_default.timeout > 500 && _default.timeout < 10000) {
            //判断xhr对象是否有ontimeout属性，没有的话就是低版本ie
            if ('ontimeout' in xhr) {
                //xhr注册超时执行的函数
                xhr.ontimeout = function () {
                    _promise.onerror()
                };
                //设置超时毫秒数
                xhr.timeout = _default.timeout;
            } else {
                //自定义超时函数
                setTimeout(function () {
                    //超时毫秒数已过，并且http事务也未完成
                    if (xhr.readyState != 4) {
                        //强制终止http事务
                        xhr.abort();
                        //执行错误函数
                        _promise.onerror();
                    }
                }, _default.timeout);
            }
        }
        //把promise返回，用于链式调用
        return _promise;
    };
    //兼容json对象
    var JSONParse = function (args) {
        if (window.JSON) {
            return JSON.parse(args)
        }
        return eval('(' + args + ')');
    };
    //生成一个promise对象
    var promise = function () {
        this.status = "default";
        this.isPromise = true;
        this.onsuccess = this.onerror = function () {
        };
    };
    //注册成功时执行的函数
    promise.prototype.success = function (func) {
        if (typeof func == 'function') {
            this.onsuccess = func;
            this.status = 'ok';
        }
        return this;
    };
    //注册失败时执行的函数
    promise.prototype.error = function (func) {
        if (typeof func == 'function') {
            this.onerror = func;
            this.status = 'fail';
        }
        return this;
    }
})();
```
引用代码如下；
```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="ajax_widget.js"></script>
    <script>
        window.onload=function(){
            aa.ajax({
                url:"/ajax",
                data:{name:"jeams bond"},
                cache:false,
                header:{"self_header":"myHeader"},
                timeout:1000,
                type:"get",
                async:true,
                dataType:'text'
            }).success(function(data){
                document.body.innerHTML+=data;
            }).error(function(err){
                console.error(err)
            });
        }
    </script>
</head>
<body>
</body>
</html>
```