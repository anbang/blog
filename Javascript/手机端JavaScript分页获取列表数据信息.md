下来刷新是常见的需求；

需要引入
```javascript
var juicer = require("juicer");
var scrollLoad = require("scrollLoad");
```
核心代码
```javascript
loadingMore: function () {
    var deviceLoad = new scrollLoad({
        element: "#myscore-list",
        loadDate: function (that) {
            var self = this;
            this.processing = true;
            var $that = $(that);
            var page = $that.data("page") || continuation;
            var pagesize = 10;
    
            var ajax = {
                url: "/point/api_getassetstradeloglist",
                type: "GET",
                cache: false,
                dataType: "json",
                data: {
                    assetsTypes:assetsType,
                    merchantId:merchantId,
                    continuation: page,
                    size: 10
                },
                success: function (data) {
                    data ={
                        "continuation": "822160249934205018",
                        "data": [
                            {
                                "tradeId": "822160249934205018",
                                "value": "+5",
                                "createTime": "2017-07-13 10:42:26",
                                "remark": "每日登陆奖励",
                                "action": 1
                            },
                            {
                                "tradeId": "822160249934205018",
                                "value": "+5",
                                "createTime": "2017-07-13 10:42:26",
                                "remark": "每日登陆奖励",
                                "action": 1
                            }
                        ],
                        "success": true,
                        "msg": null,
                        "errorCode": null
                    };
                    self.processing = false;
                    /*debug.log(data)*/
                    if (data.success) {
                        /*var nextPage = page + 1;
                        $that.data("page", nextPage);*/
                        var nextContinuation = data.continuation;
                        $that.data("page", nextContinuation);
                        $that.find(".mui-list-con").append(listTpl.render(data));
                        self.loadHelper('');
                        //如果没有更多数据
                        // if (Math.ceil(data.count / pagesize) === page) {
                        if (data.data.length<10) {
                            console.log("1111");
                            self.loadHelper('<span class="loading-tip"><i class="iconfont">&#xe66c;</i></span> 没有更多数据了');
                            self.finish = true;
                        }
                    }
    
                },
                error: function () {
                    self.processing = false;
                }
            };
            setTimeout(function () {
                $.ajax(ajax);
            }, 500)
        }
    });
    if ($myscoreList.data("count") < 10) {
        deviceLoad.finish = true;
        }
}
```

需要默认执行 loadingMore ；

模板信息

```javascript
var listTpl = juicer(['{@each data as item,index}',
    '<div class="list-item">',
    '    <span class="list-leftcon">',
    '{@if item.action === 2 }',
    '        <i class="iconfont mui-txt-warning">&#xe660;</i>',
    '{@else}',
    '        <i class="iconfont mui-txt-success">&#xe65f;</i>',
    '{@/if}',
    
    '    </span>',
    '    <div class="list-con">',
    '        <div class="list-tit">${item.remark}</div>',
    '        <p class="list-txt">${item.createTime}</p>',
    '    </div>',
    '    <div class="list-arrow">',
    '{@if item.action === 2 }',
    '        <div class="sum-num mui-txt-warning">${item.value}</div>',
    '{@else}',
    '        <div class="sum-num mui-txt-success">${item.value}</div>',
    '{@/if}',
    
    
    '    </div>',
    '</div>',
    '{@/each}'].join(""));
```

后台C#套的，参考

```
@if (Model.MyPointTradeLogList != null && Model.MyPointTradeLogList.Count > 0)
{
    if (ViewBag.IsNoMoreData)
    {
        <div class="loading">
            <span class="loading-tip"><i class="iconfont">&#xe66c;</i></span> 没有更多数据了
        </div>
    }
    else
    {
        <div class="loading"><i class="iconfont icon-lg icon-spin">&#xe622;</i> 数据加载中...</div>
    }
}
else
{
    <div class="wellpay-list-none">
        <div class="iconfont">&#xe6c1;</div>
        暂无数据
    </div>
}
```