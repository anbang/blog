最近看到BM说要搞银河系通用货币，TPS可以打到千万！感觉这家伙牛逼吹的越来越狠了。。。

记得以前EOS说是要到百万，最终线上TPS最高才3000;

而且这个数据也是作假的，估计弄高TPS的数据，多亏github上托管的，可以穿梭到当时的版本

这是最开始的仓库：https://github.com/CryptoLions/EOS-Network-monitor

在这个仓库里可以看到耍赖的TPS算法；文件是`EOS-Network-monitor/netmon-backend/src/routines/handleBlock/findMaxInfo.js`

现在EOS这个项目切换为两个项目了，新项目没有这些commit log；

    https://github.com/CryptoLions/EOS-Network-Monitor-back
    https://github.com/CryptoLions/EOS-Network-Monitor-front

而且新代码也是耍赖算法的

老代码的TPS是

```javascript
const getActionsCount = block => {
  if (block.transactions.length < 1) {
    return 0;
  }
  return block.transactions.reduce(
    (result, transaction) => result + (transaction.trx.transaction ? transaction.trx.transaction.actions.length : 0),
    0,
  );
};

const findMaxInfo = ({ current = { transactions: [] }, previous = {}, max_tps = 0, max_aps = 0 }) => {
  const SECOND = 1000;
  const currentTs = Date.parse(current.timestamp);
  const previousTs = Date.parse(previous.timestamp);
  if (currentTs === previousTs) {
    return undefined;
  }
  let live_tps;
  let live_aps;
  if (currentTs - previousTs >= SECOND) {
    live_tps = current.transactions.length;
    live_aps = getActionsCount(current);
  } else {
    live_tps = current.transactions.length + (previous.transactions ? previous.transactions.length : 0);
    live_aps = getActionsCount(current) + (previous.transactions ? getActionsCount(previous) : 0);
  }

  const res = {};
  if (live_tps > max_tps) {
    res.max_tps = live_tps;
    res.max_tps_block = current.block_num;
  }
  if (live_aps > max_aps) {
    res.max_aps = live_aps;
    res.max_aps_block = current.block_num;
  }
  if (res.max_aps || res.max_tps) {
    return res;
  }
  return undefined;
};

module.exports = findMaxInfo;
```

新代码是的是

```javascript
/* eslint-disable no-param-reassign */
const getActionsCount = require('./getActionsCount');
const { createEosApi } = require('../../helpers');
const { SECOND } = require('../../constants');

const eosApi = createEosApi();

const findMaxInfo = async ({ current = { transactions: [] }, previous, max_tps = 0, max_aps = 0 }) => {
  if (!previous || !previous.block_num) {
    previous = await eosApi.getBlock(current.block_num - 1);
  }
  const currentTs = Date.parse(current.timestamp);
  const previousTs = Date.parse(previous.timestamp);
  if (currentTs === previousTs) {
    return undefined;
  }
  let live_tps;
  let live_aps;
  // the block was produced in one second or more
  if (currentTs - previousTs >= SECOND) {
    const transactionsNumber = current.transactions.length;
    const actionsNumber = getActionsCount(current);
    const producedInSeconds = (currentTs - previousTs) / SECOND;
    live_tps = transactionsNumber / producedInSeconds;
    live_aps = actionsNumber / producedInSeconds;
  } else {
    // the block was produced in half of second
    // find number of transactions for 0.5 sec for previous block
    if (!previous.producedInSeconds) {
      const beforePrevious = await eosApi.getBlock(previous.block_num - 1);
      previous.producedInSeconds = (Date.parse(previous.timestamp) - Date.parse(beforePrevious.timestamp)) / SECOND;
    }
    const previousTransactionsNumber = current.transactions.length;

    live_tps = current.transactions.length + (previousTransactionsNumber / previous.producedInSeconds / 2);
    live_aps = getActionsCount(current) + (getActionsCount(previous) / previous.producedInSeconds / 2);
  }
  live_aps = live_aps < live_tps ? live_tps : live_aps;
  const res = {};
  if (live_tps > max_tps) {
    res.max_tps = live_tps;
    res.max_tps_block = current.block_num;
  }
  if (live_aps > max_aps) {
    res.max_aps_block = current.block_num;
    res.max_aps = live_aps;
  }
  if (res.max_aps || res.max_tps) {
    return res;
  }
  return undefined;
};

module.exports = findMaxInfo;
```

老代码的TPS核心计算是
```javascript
//判断当前块与上一个块的出块时间差值，
 if (currentTs - previousTs >= SECOND) {
     //如果大于等于1秒,执行这里
     //假如 块1000 与 块999 出块时间差值为6s，块1000里有3000笔交易，那么TPS取值是3000了，（而不是取3000/6），这明显是不要脸的算法
    live_tps = current.transactions.length;
  } else {
      //如果小于1s执行这里
      //假设块1001与块1000差值是0.5秒，假设块1001里有2000笔交易，块1000里有3000笔交易，那么取值是2000+3000=5000的TPS；（这个算法也是不合理）
    live_tps = current.transactions.length + (previous.transactions ? previous.transactions.length : 0);
  }

  const res = {};
  //以下代码是是：如果当前1001块计算出的TPS结果5000大于数据库储存的曾经最大TPS，则储存5000为最大Tps，如果不大于曾经储存的TPS，就不会更新
  if (live_tps > max_tps) {
    res.max_tps = live_tps;
    res.max_tps_block = current.block_num;
  }
  if (res.max_aps || res.max_tps) {
    return res;
  }
  return undefined;
```

新版代码的TPS计算核心

```javascript
  if (currentTs - previousTs >= SECOND) {
    const transactionsNumber = current.transactions.length;
    const producedInSeconds = (currentTs - previousTs) / SECOND;
    //这个的新算法已经改正了，假如 块1000 与 块999 出块时间差值为6s，块1000里有3000笔交易，那么TPS取值是 3000/6 了
    live_tps = transactionsNumber / producedInSeconds;
  } else {
    // the block was produced in half of second
    // find number of transactions for 0.5 sec for previous block
    if (!previous.producedInSeconds) {
      const beforePrevious = await eosApi.getBlock(previous.block_num - 1);
      previous.producedInSeconds = (Date.parse(previous.timestamp) - Date.parse(beforePrevious.timestamp)) / SECOND;
    }
    const previousTransactionsNumber = current.transactions.length;
    //这里取值是 （当前的Block里面的交易数量）+（上一个块交易数量/(上一个时间戳-上上的时间戳 等到的秒数)/2）
    live_tps = current.transactions.length + (previousTransactionsNumber / previous.producedInSeconds / 2);
  }
```
新版算法里，大于1s的算法很合理，但是小与1秒的算法还是不合理，这种算法得到的结果是范围波动太大；会偏离实际值更大或者更小；

算法DEMO演算，演示跑100个块得到的结果
```javascript
let MAX = 1500,
    MIN = 500;
let target = [];
for (let k=0;k<100;k++){
    let database=[];
    let temp;//临时数据
    let timeFlag=0;
    for (let i=0;i<6;i++){
        temp={
            timestamp : Math.floor(Math.random()*(MAX-MIN)+MIN)+timeFlag,
            transaction_num: 100
        };
        timeFlag = temp.timestamp;
        database.push(temp);
    }
    // console.log(database);
    let C = database[database.length-1];
    let B = database[database.length-2];
    let A = database[database.length-3];
    let liveTps1;
    let liveTps2;
    let Diff = C.timestamp - B.timestamp;
    if(Diff>=1000){

    }else{
        let tempDiff  = (B.timestamp - A.timestamp)/1000;
        liveTps1 = C.transaction_num + (C.transaction_num/ tempDiff / 2);
        liveTps2 = C.transaction_num / (Diff /1000);
    }


    let tempTarget={
        eos : liveTps1,
        czr: liveTps2
    };
    target.push(tempTarget);
}

console.log(target);
let EOSMAX=0,
    EOSMIN=10000;
let CZRMAX=0,
    CZRMIN=10000;
target.forEach(item=>{
    //取最大TPS 取最小TPS
    if(item.eos>=EOSMAX){
        EOSMAX = item.eos;
    }
    if(item.eos<=EOSMIN){
        EOSMIN = item.eos;
    }

    //CZR
    if(item.czr>=CZRMAX){
        CZRMAX = item.eos;
    }
    if(item.czr<=CZRMIN){
        CZRMIN = item.eos;
    }
});
console.log(`EOSMAX:${EOSMAX},EOSMIN:${EOSMIN}`);
console.log(`CZRMAX:${CZRMAX},CZRMIN:${CZRMIN}`);
```

这种算法得到的结果是范围波动太大；会偏离实际值更大或者更小；但是EOS是只统计曾经出现的最高值；