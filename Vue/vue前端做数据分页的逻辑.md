vue分页时缓存已获取的数据

### 注意：这是一种前端做分页的行为，一般不会这么干，因为项目特殊，所以这样处理了

走RPC接口，区块链的节点程序那里拿数据,节点是在链上同步获取数据的

**场景**：前端获取后端来的第一条数据，获取第一条后存起来，然后用户点击下一页时候，一次获取对应数据，并把以获取的数据存起来；当从`第三页`点击`上一页`到`第二页`时候，直接从储存的数据里读取，就不用再请求RPC接口了,

**注意事项**：这时候分页不显示页码，分页按钮只显示`上一页`和`第二页`，因为如果有页码的话，前端并不知道怎么截取数据;

代码如下

```vue
<template>
    <div class="page-block">
        <header-cps></header-cps>
        <div class="block-info-wrap">
            <div class="container">
                <search></search>
                <div class="bui-dlist">
                    <div class="block-item-des">
                        <strong class="bui-dlist-tit">账号
                            <span class="space-des"></span>
                        </strong>
                        <div class="bui-dlist-det">{{account}}</div>
                    </div>
                    <div class="block-item-des">
                        <strong class="bui-dlist-tit">余额
                            <span class="space-des"></span>
                        </strong>
                        <div class="bui-dlist-det">{{balance | toCZRVal}} CZR</div>
                    </div>
                </div>
                <div class="account-content">
                    <h2 class="transfer-tit">交易记录</h2>
                    <div class="transfer-log" v-loading="loadingSwitch">
                        <template v-for="item in accountInfo.currentTxList">
                            <div v-if="item.to == accountInfo.address">
                                <router-link class="transfer-item plus-assets" :to="'/block/'+ item.hash">
                                    <div class="transfer-info">
                                        <p class="by-address">{{item.from}}</p>
                                        <p class="transfer-time">{{item.exec_timestamp |toDate }}</p>
                                    </div>
                                    <div class="transfer-assets">
                                        <div class="assets">+ {{item.amount | toCZRVal }} CZR</div>
                                    </div>
                                </router-link>
                            </div>
 
                            <div v-if="item.from == accountInfo.address">
                                <router-link class="transfer-item less-assets " :to="'/block/'+ item.hash">
                                    <div class="transfer-info">
                                        <p class="by-address">{{item.to}}</p>
                                        <p class="transfer-time">{{item.exec_timestamp |toDate }}</p>
                                    </div>
                                    <div class="transfer-assets">
                                        <div class="assets">- {{item.amount | toCZRVal }} CZR</div>
                                    </div>
                                </router-link>
                            </div>
                        </template>
                        <div class="pagin-wrap b-flex b-flex-justify" v-if="accountInfo.tx_list.length>=pagingSwitch.limit">
                            <el-button :disabled="pagingSwitch.beforeDisabled" @click="beforeList" class="before-btn">上一页</el-button>
                            <el-button :disabled="pagingSwitch.nextDisabled" @click="nextList" class="next-btn">下一页</el-button>
                        </div>
                    </div>
 
                    <!--  No transaction record  -->
                    <div v-if="accountInfo.tx_list.length==0" class="no-transfer-log">
                        <i class="el-icon-document iconfont"></i>
                        <p class="no-list">暂无交易记录</p>
                    </div>
                </div>
            </div>
 
        </div>
 
    </div>
</template>
 
<script>
import HeaderCps from "@/components/Header/Header";
import Search from "@/components/Search/Search";
let self = null;
 
export default {
    name: "Block",
    components: {
        HeaderCps,
        Search
    },
    data() {
        return {
            account: this.$route.params.id,
            balance: 0,
            pagingSwitch: {
                limit: 10,
                beforeDisabled: true,
                nextDisabled: false
            },
            accountInfo: {
                address: this.$route.params.id,
                tx_list: [],
                currentTxList: []
            },
            transactionInfo: null,
            loadingSwitch: true,
            txStatus: "-",
            lastBlockHash: ""
        };
    },
    created() {
        self = this;
        this.initDatabase();
        this.initTransactionInfo();
        self.getTxList();
    },
    methods: {
        initTransactionInfo() {
            this.transactionInfo = {
                hash: "", //哈希值
                from: "",
                to: "",
                amount: "",
                previous: "",
                parents: "",
                witness_list: "",
                witness_list_block: "",
                last_summary: "",
                last_summary_block: "",
                data: "",
                exec_timestamp: "",
                signature: ""
            };
        },
        initDatabase() {
            var self = this;
            self.$czr.request
                .accountBalance(self.account)
                .then(function(data) {
                    return data;
                })
                .then(function(data) {
                    self.balance = data.balance;
                });
        },
 
        // 列表
        //get List
        getTxList() {
            self.$czr.request
                .blockList(
                    self.accountInfo.address,
                    self.pagingSwitch.limit,
                    self.lastBlockHash
                )
                .then(function(data) {
                    if (data.error) {
                        self.$message({
                            message: data.error,
                            type: "error"
                        });
                        self.loadingSwitch = false;
                        return;
                    }
 
                    data = !!data ? data : { list: [] };
                    self.loadingSwitch = false;
                    if (data.list.length > 0) {
                        self.accountInfo.currentTxList = data.list;
                        data.list.forEach(element => {
                            self.accountInfo.tx_list.push(element);
                        });
                    }
 
                    //是否有下页
                    if (data.list.length < self.pagingSwitch.limit) {
                        self.pagingSwitch.nextDisabled = true;
                    } else {
                        self.pagingSwitch.nextDisabled = false;
                        self.lastBlockHash =
                            data.list[data.list.length - 1].hash; //需要拿新的HASH，准备下次请求
                    }
                });
        },
        getNextList() {
            /* 
                currentTxList最后一个hash是否 等于 tx_list 最后一个hash；
                - 等于   需要获取
                - 不等   从tx_list中获取
            */
            var _currentTxList = self.accountInfo.currentTxList;
            var currentTxListHashBlock =
                _currentTxList[_currentTxList.length - 1].hash;
            var _txList = self.accountInfo.tx_list;
            var lastTxListHashBlock = _txList[_txList.length - 1].hash;
            if (currentTxListHashBlock == lastTxListHashBlock) {
                //获取
                self.lastBlockHash = lastTxListHashBlock;
                self.getTxList();
            } else {
                //不获取
                //先把当前的哈希，替换为下一个hash
                var startHash = currentTxListHashBlock;
                var tempAry = [];
                var ele;
                for (var j = 0; j < _txList.length; j++) {
                    ele = _txList[j];
                    var _isSet = ele.hash == startHash;
                    var _isGoOn = tempAry.length < self.pagingSwitch.limit;
                    if (_isSet && _isGoOn) {
                        //如果是最后一个item，就不要循环了
                        if (
                            _txList[j].hash == _txList[_txList.length - 1].hash
                        ) {
                            self.pagingSwitch.nextDisabled = true; //释放 后翻
                            break;
                        }
                        startHash = _txList[j + 1].hash;
                        tempAry.push(_txList[j + 1]);
                    }
                }
                // _txList.forEach ((ele, index) => {
                //     var _isSet = ele.hash == startHash;
                //     var _isGoOn = tempAry.length < self.pagingSwitch.limit;
                //     if (_isSet && _isGoOn) {
                //         startHash = _txList[index + 1].hash;
                //         tempAry.push(_txList[index + 1]);
                //     }
                //     //如果是最后一个item，就不要循环了
                //     if(_txList[_txList.length-1]==startHash){
                //         return;
                //     }
                // });
 
                self.loadingSwitch = false;
                self.accountInfo.currentTxList = tempAry;
                self.lastBlockHash = tempAry[tempAry.length - 1].hash;
            }
        },
        getBeforeList() {
            //TODO 从 tx_list 中取一些值给currentTxLList
            /* 
            1.当前有 lastBlockHash 开始循环找
            2.可以取值，并且可以继续，则取值
                - 取值同时，设置l astBlockHash 为前一个Hash,如果是第一个item了，则astBlockHash 为""
 
            = 开始 > 结束   返回
            = 开始 > 中间   返回
            */
            var localList = self.accountInfo.tx_list;
            var targetAry = [];
            if (self.lastBlockHash) {
                //当前list最后数据的hashBlock 就是当前hash；这时候需要换,换成上一页最后一个hash；
                var _currentTxList = self.accountInfo.currentTxList;
                var currentTxListHashBlock =
                    _currentTxList[_currentTxList.length - 1].hash;
                if (self.lastBlockHash == currentTxListHashBlock) {
                    //当前 lastBlockHash 在 tx_list 中的索引
                    var currentIndexInTxLixt = 0;
                    localList.forEach((ele, index) => {
                        if (ele.hash == self.lastBlockHash) {
                            currentIndexInTxLixt = index;
                        }
                    });
                    var currentTxLeng = self.accountInfo.currentTxList.length;
                    var lessNum =
                        self.pagingSwitch.limit > currentTxLeng
                            ? currentTxLeng
                            : self.pagingSwitch.limit;
                    var targetIndex = currentIndexInTxLixt - lessNum;
                    if (targetIndex < 0) {
                        self.pagingSwitch.beforeDisabled = true;
                        self.loadingSwitch = false;
                        return;
                    }
 
                    // var targetIndex = currentIndexInTxLixt - self.pagingSwitch.limit;
                    self.lastBlockHash = localList[targetIndex].hash;
                }
 
                //如果有lastBlockHash
                for (var i = localList.length - 1; i >= 0; i--) {
                    //如果当前Hash 等于 循环的哈希,开始取值
                    var isSet = localList[i].hash == self.lastBlockHash; //是否取值
                    var isGoOn = targetAry.length < self.pagingSwitch.limit; //是否继续取值
                    if (isSet && isGoOn) {
                        targetAry.unshift(localList[i]);
                        self.lastBlockHash = i > 0 ? localList[i - 1].hash : "";
                        if (i == 0) {
                            self.pagingSwitch.beforeDisabled = true;
                        }
                    }
                    //如果数据读取完毕，不需要循环
                    if (!isGoOn) {
                        break;
                    }
                }
            }
            if (targetAry.length > 0) {
                self.accountInfo.currentTxList = targetAry;
            } else {
                //不能上翻
                self.pagingSwitch.beforeDisabled = true;
            }
            self.loadingSwitch = false;
        },
        nextList() {
            self.loadingSwitch = true;
            self.getNextList();
            self.pagingSwitch.beforeDisabled = false; //释放 前翻
        },
        beforeList() {
            self.loadingSwitch = true;
            self.getBeforeList();
            self.pagingSwitch.nextDisabled = false; //释放 后翻
        }
    },
    filters: {
        toCZRVal: function(val) {
            let tempVal = self.$czr.utils.fromWei(val, "czr");
            return tempVal; //TODO Keep 4 decimal places
        },
        toDate: function(val) {
            if (val == "0" || !val) {
                return "-";
            }
            let newDate = new Date();
            newDate.setTime(val * 1000);
            let addZero = function(val) {
                return val < 10 ? "0" + val : val;
            };
            return (
                newDate.getFullYear() +
                " / " +
                addZero(newDate.getMonth() + 1) +
                " / " +
                addZero(newDate.getDate()) +
                " " +
                addZero(newDate.getHours()) +
                ":" +
                addZero(newDate.getMinutes()) +
                ":" +
                addZero(newDate.getSeconds())
            );
        }
    }
};
</script>
 
<style   scoped>
.page-block {
    width: 100%;
    position: relative;
}
#header {
    color: #fff;
    background: #5a59a0;
}
 
.block-info-wrap {
    position: relative;
    width: 100%;
    margin: 0 auto;
    color: black;
    text-align: left;
    padding-top: 20px;
    padding-bottom: 80px;
}
.block-item-des {
    padding: 10px 0;
    border-bottom: 1px dashed #f6f6f6;
}
@media (max-width: 1199px) {
    .bui-dlist {
        color: #3f3f3f;
        font-size: 16px;
        line-height: 2.4;
    }
    .block-item-des {
        display: block;
    }
    .bui-dlist-tit {
        display: block;
        width: 100%; /* 默认值, 具体根据视觉可改 */
        margin: 0;
        text-align: left;
    }
    .bui-dlist-det {
        display: block;
        color: #5f5f5f;
        text-align: left;
        margin: 0;
        table-layout: fixed;
        word-break: break-all;
        overflow: hidden;
    }
}
 
@media (min-width: 1200px) {
    .bui-dlist {
        color: #3f3f3f;
        font-size: 16px;
        line-height: 2.4;
        margin-top: 20px;
        border-top: 1px dashed #f6f6f6;
    }
    .block-item-des {
        display: -webkit-box;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
    }
    .bui-dlist-tit {
        float: left;
        width: 20%; /* 默认值, 具体根据视觉可改 */
        text-align: right;
        margin: 0;
    }
    .bui-dlist-det {
        float: left;
        color: #5f5f5f;
        width: 80%; /* 默认值，具体根据视觉可改 */
        text-align: left;
        margin: 0;
        table-layout: fixed;
        word-break: break-all;
        overflow: hidden;
    }
}
 
.txt-warning {
    color: #e6a23c;
}
.txt-info {
    color: #909399;
}
.txt-success {
    color: #67c23a;
}
.txt-danger {
    color: #f56c6c;
}
 
.bui-dlist-tit .space-des {
    display: inline-block;
    width: 10px;
}
 
/*  记录 */
 
.account-content {
    text-align: left;
    margin-top: 40px;
}
.account-content .transfer-tit {
    font-size: 18px;
    font-weight: 400;
}
 
/* Transaction Record */
.account-content .no-transfer-log {
    text-align: center;
    color: #9b9b9b;
}
.account-content .no-transfer-log .iconfont {
    font-size: 128px;
}
.account-content .transfer-log {
    padding: 22px 0;
}
 
.transfer-log .transfer-item {
    background-color: #fff;
    padding: 10px 0;
    cursor: pointer;
    border-bottom: 1px dashed #f0f0f0;
    -webkit-user-select: none;
}
.transfer-log .transfer-item:hover {
    text-decoration: none;
    background-color: #f5f5f5;
}
 
@media (max-width: 1199px) {
    .transfer-log .transfer-item {
        display: block;
    }
    .transfer-time {
        padding: 10px 0;
    }
}
 
@media (min-width: 1200px) {
    .transfer-log .transfer-item {
        display: -webkit-box;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
    }
    .account-content .transfer-log .transfer-info {
        width: 800px;
        padding-left: 10px;
        text-align: left;
    }
    .transfer-log .transfer-assets .assets {
        font-size: 18px;
        height: 42px;
        line-height: 42px;
        width: 300px;
        text-align: right;
    }
}
 
.transfer-log .icon-wrap {
    width: 42px;
    height: 42px;
    border-radius: 50%;
}
.transfer-log .icon-wrap .icon-transfer {
    color: #fff;
    position: relative;
    left: 11px;
    top: 4px;
    font-size: 20px;
}
.transfer-log .plus-assets .icon-wrap {
    background-color: rgba(0, 128, 0, 0.555);
}
.transfer-log .less-assets .icon-wrap {
    background-color: rgba(255, 153, 0, 0.555);
}
.transfer-log .by-address {
    width: 100%;
    color: #9a9c9d;
    table-layout: fixed;
    word-break: break-all;
    overflow: hidden;
    color: rgb(54, 54, 54);
}
.transfer-log .transfer-time {
    color: rgb(161, 161, 161);
}
 
.plus-assets .assets {
    color: green;
}
.less-assets .assets {
    color: rgb(255, 51, 0);
}
.iconfont {
    font-size: 18px;
    color: #bfbef8;
}
.no-list {
    padding-top: 20px;
}
.pagin-wrap {
    padding: 15px 0;
}
</style>
```