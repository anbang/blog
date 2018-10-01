原因：QQ群的一个好友问了一个问题，看不懂某一段代码什么意思；

代码如下；

```javascript
    getYhjInit:function () {
        var that = this;
        $.ajax({
            type:'post',
            url:api+'api/user/pc/listUserCoupom.do',
            data:{
                token:that.tokens
            },
            success:function (res) {
                // 未使用
                for(var i= 0;i<res.response.unUserdMap.unUserdList.length;i++){
                    res.response.unUserdMap.unUserdList[i].beforetime  = webUizjx.timeData({
                        timestamp:Number(res.response.unUserdMap.unUserdList[i].startTime)/1000
                    })
                    res.response.unUserdMap.unUserdList[i].aftertime  = webUizjx.timeData({
                        timestamp:Number(res.response.unUserdMap.unUserdList[i].endTime)/1000
                    })
                }
                that.list = res.response.unUserdMap;
                console.log(that.list);
                // 已使用
                for(var i= 0;i<res.response.userdMap.userdList.length;i++){
                    res.response.userdMap.userdList[i].beforetime  = webUizjx.timeData({
                        timestamp:Number(res.response.userdMap.userdList[i].startTime)/1000
                    })
                    res.response.userdMap.userdList[i].aftertime  = webUizjx.timeData({
                        timestamp:Number(res.response.userdMap.userdList[i].endTime)/1000
                    })
                }
                that.readyList = res.response.userdMap;
                // 已过期
                for(var i= 0;i<res.response.timeoutMap.timeoutList.length;i++){
                    res.response.timeoutMap.timeoutList[i].beforetime  = webUizjx.timeData({
                        timestamp:Number(res.response.timeoutMap.timeoutList[i].startTime)/1000
                    })
                    res.response.timeoutMap.timeoutList[i].aftertime  = webUizjx.timeData({
                        timestamp:Number(res.response.timeoutMap.timeoutList[i].endTime)/1000
                    })
                }
                that.yiguoqi = res.response.timeoutMap;
            }
        })
    },
```

（上面的代码的属性名getYhjInit 原本是getYhj，为了区分，所以加了Init）

感觉这个代码非常的糟糕，

所以我做了一下代码重构；

整理后，就有利于思路的整理了；

```javascript
    getYhj: function () {
        var that    = this;
        var options = {
            type: 'post',
            url: api + 'api/user/pc/listUserCoupom.do',
            data: {token: that.tokens},
            success: function (res) {
                var MODIFIER   = 1000;
                //赋值that，方面别的方法调用；
                that.list      = res.response.unUserdMap;
                that.readyList = res.response.userdMap;
                that.yiguoqi   = res.response.timeoutMap;
     
                var resUnUserdList = that.list.unUserdList,//未使用
                    resUserdList   = that.readyList.userdList,//已使用
                    resTimeoutList = that.yiguoqi.timeoutList;//已过期
     
                // 未使用      目标是把处理后的时间挂在that.list上;方便别的方法调用that.list的属性；
                for (var i = 0, iLen = resUnUserdList.length; i < iLen; i++) {
                    resUnUserdList[i].beforetime = webUizjx.timeData({timestamp: Number(resUnUserdList[i].startTime) / MODIFIER});
                    resUnUserdList[i].aftertime  = webUizjx.timeData({timestamp: Number(resUnUserdList[i].endTime) / MODIFIER});
                }
     
                // 已使用
                for (var j = 0, jLen = resUserdList.length; j < jLen; j++) {
                    resUserdList[j].beforetime = webUizjx.timeData({timestamp: Number(resUserdList[j].startTime) / MODIFIER});
                    resUserdList[j].aftertime  = webUizjx.timeData({timestamp: Number(resUserdList[j].endTime) / MODIFIER});
                }
     
                // 已过期
                for (var k = 0, kLen = resTimeoutList.length; k < kLen; k++) {
                    resTimeoutList[k].beforetime = webUizjx.timeData({timestamp: Number(resTimeoutList[k].startTime) / MODIFIER});
                    resTimeoutList[k].aftertime  = webUizjx.timeData({timestamp: Number(resTimeoutList[k].endTime) / MODIFIER});
                }
            }
        };
        $.ajax(options);
    }
```