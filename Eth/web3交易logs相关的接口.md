- getPastLogs
- myContract.getPastEvents()
- web3.eth.abi.decodeLog
- subscribe（'logs'）- 未实现

`getPastLogs` 和 `myContract.getPastEvents()` 都是调用`eth_getLogs`

## getPastLogs

```
web3.eth.getPastLogs(options [, callback])

new Method({
    name: 'getPastLogs',
    call: 'eth_getLogs',
    params: 1,
    inputFormatter: [formatter.inputLogFormatter],
    outputFormatter: formatter.outputLogFormatter
}),

```

获取过去的日志，匹配给定的选项。

### 参数

\<Object> - 过滤器选项如下：

- `fromBlock` - Number|String：
  - 从哪个块开始查询
  - 默认"latest"
  - "latest"可以表示最近和"pending"当前挖掘的块。
- `toBlock` - Number|String：
  - 终止的块
  - 默认"latest"。
- `address` - String|Array：
  - 仅从特定帐户获取日志的地址或地址列表。
- `topics` - Array：
  - 必须各自出现在日志条目中的值数组。
  - 如果你想让主题不被使用 null，顺序很重要，
    - 例如。您还可以为每个主题传递一个数组，其中包含该主题的选项；
    - 例如 `[null, '0x12...']null, ['option1', 'option2']]`

### 返回

`Promise` returns Array- 日志对象的数组。

返回事件的结构 Object 在 Array 如下外观：

- `address` - String：此事件源于此。
- `data` - String：包含非索引日志参数的数据。
- `topics` - Array：具有最多 4 个 32 字节主题的数组，主题 1-3 包含日志的索引参数。
- `logIndex` - Number：块中事件索引位置的整数。
- `transactionIndex`- Number：事务的索引位置的整数，事件是在中创建的。
- `transactionHash` 32 字节 - String：此事件创建的事务的哈希值。
- `blockHash` 32 字节 - String：创建此事件的块的哈希值。null 当它仍然未决时。
- `blockNumber` - Number：创建此日志的块编号。null 当仍处于挂起状态时。

### 示例

```

web3.eth.getPastLogs(
        {
            address: "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe",
            topics: ["0x033456732123ffff2342342dd12342434324234234fd234fd23fd4f23d4234"]
        }
    )
    .then(console.log);

> [
    {

        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: [
            '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
            '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385'
            ]
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'

    },
    {...}
  ]

```

## web3.eth.abi.decodeLog

解码 ABI 编码的日志数据。

```
web3.eth.abi.decodeLog(inputs, hexString, topics);
```

### 参数

- `inputs` - Object：JSON 接口输入数组。
- `hexString` - String：日志 data 字段中的 ABI 字节代码。
- `topics` - Array：具有日志的索引参数主题的数组，如果是非匿名事件，则不带主题`[0]`，否则使用主题`[0]`。

### 返回

Object - 包含已解码参数的结果对象。

### 示例

```
web3.eth.abi.decodeLog([{
    type: 'string',
    name: 'myString'
},{
    type: 'uint256',
    name: 'myNumber',
    indexed: true
},{
    type: 'uint8',
    name: 'mySmallNumber',
    indexed: true
}],
'0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000748656c6c6f252100000000000000000000000000000000000000000000000000',
['0x000000000000000000000000000000000000000000000000000000000000f310', '0x0000000000000000000000000000000000000000000000000000000000000010']);


> Result {
    '0': 'Hello%!',
    '1': '62224',
    '2': '16',
    myString: 'Hello%!',
    myNumber: '62224',
    mySmallNumber: '16'
}
```

## subscribe（'logs'）

订阅传入日志，按给定选项进行过滤。

```
web3.eth.subscribe('logs', options [, callback]);
```

### 参数

- `"logs"`- String，订阅的类型。
- Object - 订阅选项
  - `fromBlock` - Number：最早的块数。默认情况下 null。
  - `address`- String|Array：仅从特定帐户获取日志的地址或地址列表。
  - `topics`- Array：必须各自出现在日志条目中的值数组。
    - 如果你想让主题不被使用 null，顺序很重要，
    - 例如。您还可以为每个主题传递另一个数组，其中包含该主题的选项，
    - 例如[null, '0x00...']null, ['option1', 'option2']]
- callback- Function:(可选）
  - 返回错误对象作为第一个参数，结果作为秒。将为每个传入的订阅调用。

### 返回

`EventEmitter`：订阅实例作为事件发射器，具有以下事件：

- `"data"` 返回 Object：
  - 以日志对象作为参数触发每个传入日志。
- `"changed"` 返回 Object：
  - 触发从区块链中删除的每个日志。该日志将具有附加属性。`"removed: true"`,
- "error"返回 Object：发生订阅错误时触发。

有关返回事件的结构，`Object` 参阅 `web3.eth.getPastEvents` 返回值。

### 回调返回

Object|Null - 如果订阅失败，则第一个参数是错误对象。
Object- `web3.eth.getPastEvents` 中的日志对象返回值。

### 示例

```
//subscription
var subscription = web3.eth.subscribe('logs', {
    address: '0x123456..',
    topics: ['0x12345...']
}, function(error, result){
    if (!error){
        console.log(result);
    }
})
.on("data", function(log){
    console.log(log);
})
.on("changed", function(log){
    //....
});

// unsubscribes
subscription.unsubscribe(function(error, success){
    if(success){
        console.log('Successfully unsubscribed!');
    }
});
```

## myContract.getPastEvents()

```
myContract.getPastEvents(event[, options][, callback])

var getPastLogs = new Method({
    name: 'getPastLogs',
    call: 'eth_getLogs',
    params: 1,
    inputFormatter: [formatters.inputLogFormatter],
    outputFormatter: this._decodeEventABI.bind(subOptions.event)
});

```

获取此合约的过去事件。

### 参数

- `event`- String：合同中事件的名称，或"allEvents"获取所有事件。
- `options`- Object（可选）：用于部署的选项。
  - `filter` - Object（可选）：允许您通过索引参数过滤事件，
    - 例如，表示“myNumber”为 12 或 13 的所有事件。`{filter: {myNumber: [12,13]}}`
  - `fromBlock`- Number（可选）：从中获取事件的块编号。
  - `toBlock`- Number（可选）：用于获取事件的块编号（默认为"latest"）。
  - `topics`- Array（可选）：这允许手动设置事件过滤器的主题。
    - 如果给定过滤器属性和事件签名，则不会自动设置（`topic [0]`）。
- `callback`- Function（可选）：此回调将以第二个参数的事件日志数组或第一个参数的错误触发。

### 返回

`Promise` 返回 `Array` ：包含过去事件的数组`Objects`，与给定的事件名称和过滤器匹配。

对于返回事件的结构，Object 请参阅 getPastEvents 返回值。

### 示例

```

myContract.getPastEvents('MyEvent', {
    filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
    fromBlock: 0,
    toBlock: 'latest'
}, function(error, events){ console.log(events); })
.then(function(events){
    console.log(events) // same results as the optional callback above
});

> [{
    returnValues: {
        myIndexedParam: 20,
        myOtherIndexedParam: '0x123456789...',
        myNonIndexParam: 'My String'
    },
    raw: {
        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
    },
    event: 'MyEvent',
    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    logIndex: 0,
    transactionIndex: 0,
    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
    blockNumber: 1234,
    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
},{
    ...
}]

```
