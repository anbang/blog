web3.eth.Contract 模块的属性和方法

该模块封装在`web3.eth.Contract`;

使用的时候需要提供你创建合约时候返回的`json信息`，WEB3 会基于 json 文件自动转化为 RPC 级别的 ABI 调用；

这样就可以与智能合约做互动了；

# 实例化

```
let myContract = new web3.eth.Contract(jsonInterface[, address][, options])
```

```
var myContract = new web3.eth.Contract([...], '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe', {
    from: '0x1234567890123456789012345678901234567891', // default from address
    gasPrice: '20000000000' // default gas price in wei, 20 gwei in this case
});
```

# 属性

### options

```
myContract.options
```

Object - 选项：

-   address- String：部署合同的地址。请参阅 options.address。
-   jsonInterface- Array：合同的 json 接口。请参见 options.jsonInterface。
-   data- String：合同的字节代码。合同部署时使用。
-   from- String：地址交易应该来自。
-   gasPrice- String：用于交易的天然气价格。
-   gas- Number：为交易提供的最大气体（气体限制）。

### options.address

获取或者设置合约地址（获取时候为 null 则是尚未设置地址）

```
myContract.options.address;
> '0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae'

// set a new address
myContract.options.address = '0x1234FFDD...';
```

### options.jsonInterface

**这里面需要对每个方法做签名；（转成 0xXXXX 格式）**

获取或者设置合约的 json 接口。

重新设置它将重新生成合同实例的方法和事件。

```
myContract.options.jsonInterface;
> [{
    "type":"function",
    "name":"foo",
    "inputs": [{"name":"a","type":"uint256"}],
    "outputs": [{"name":"b","type":"address"}]
},{
    "type":"event",
    "name":"Event",
    "inputs": [{"name":"a","type":"uint256","indexed":true},{"name":"b","type":"bytes32","indexed":false}],
}]

// set a new interface
myContract.options.jsonInterface = [...];
```

# 方法

-   clone
-   deploy
-   methods
-   methods.myMethod.call
-   methods.myMethod.send
-   methods.myMethod.estimateGas
-   methods.myMethod.encodeABI

### clone

```
myContract.clone()
```

示例

```
var contract1 = new eth.Contract(abi, address, {gasPrice: '12345678', from: fromAddress});

var contract2 = contract1.clone();
contract2.options.address = address2;

(contract1.options.address !== contract2.options.address);
> true
```

### deploy

```
myContract.deploy({
    data: '',
    arguments: []
})
```

调用此函数将合同部署到区块链。成功部署后，承诺将通过新的合同实例解决。

```
var interfaceFile = require('./interface.json');
const HDWalletProvider = require("truffle-hdwallet-provider");
let bytecode = '0x608060405234801561001057600080fd5b506102a9806100206000396000f3fe60806040526004361061008d576000357c0100000000000000000000000000000000000000000000000000000000900480631b56fda01161006b5780631b56fda0146101345780634f9d719e1461014957806364c7133b14610151578063b819499e1461017b5761008d565b80630dbe671f146100925780630f682008146100b9578063125d9d9014610102575b600080fd5b34801561009e57600080fd5b506100a7610190565b60408051918252519081900360200190f35b3480156100c557600080fd5b506100e9600480360360408110156100dc57600080fd5b5080359060200135610196565b6040805192835260208301919091528051918290030190f35b34801561010e57600080fd5b506101326004803603604081101561012557600080fd5b50803590602001356101d8565b005b34801561014057600080fd5b5061013261021c565b610132610223565b34801561015d57600080fd5b506100a76004803603602081101561017457600080fd5b5035610258565b34801561018757600080fd5b506100a7610277565b60005481565b6000806001848154811015156101a857fe5b90600052602060002001546001848154811015156101c257fe5b9060005260206000200154915091509250929050565b60018054808201825560008290527fb10e2d527612073b26eecdfd717e6a320cf44b4afac2b0732d9fcbe2b7fa0cf690810193909355805480820190915590910155565b6001600055565b6040805134815290517f260823607ceaa047acab9fe3a73ef2c00e2c41cb01186adc4252406a47d734469181900360200190a1565b600180548290811061026657fe5b600091825260209091200154905081565b6000549056fea165627a7a7230582081e0463af8dcbfeda13f7bbdbe11dc8e87e83639f3825782a0e7918a1fe26b980029';

let rinkebyUrl = 'https://rinkeby.infura.io/uIkf4qZgOSqDV0Ir5np1';
const mnemonic = ""; // 12 word mnemonic
let provider = new HDWalletProvider(mnemonic, rinkebyUrl);

// console.log(provider);
var Web3 = require('web3');
var web3 = new Web3(provider);
// var myContract = new web3.eth.Contract(interfaceFile);
var myContract = new web3.eth.Contract(interfaceFile);

// let Contract = require('web3-eth-contract');
// Contract.setProvider(rinkebyUrl);
// let myContract = new Contract(interfaceFile);

// console.log(myContract)

// 属性
console.log(myContract.options);
console.log(myContract.options.address);

// console.log(myContract.options.address)
// console.log(myContract.options.jsonInterface)

//方法
console.log("\n方法------------------------------ ");
let myContract2 = myContract.clone();
myContract2.options.address = '0x0223fc70574214F65813fE336D870Ac47E147fAe'
console.log(myContract.options.address);
console.log(myContract2.options.address);

web3.eth.getAccounts(console.log);

myContract.deploy({
    data: bytecode
})
    .send({
        from: '0x7386445b7C0022FB0c6B08466a8E6ae4A97A134b',
        gas: 3000000,
        gasPrice: '1000000000'
    }, function (error, transactionHash) {
        console.log("error ==> ", error)
        console.log("transactionHash ==> ", transactionHash)
    })
    .on('error', function (error) {
        console.log("------ error")
        console.log(error)
    })
    //交易hash
    .on('transactionHash', function (transactionHash) {
        console.log('------ transactionHash')
        console.log(transactionHash)
    })
    //收据
    .on('receipt', function (receipt) {
        console.log('------ receipt')
        console.log(receipt)
        console.log(receipt.contractAddress)
    })
    // 确认数
    .on('confirmation', function (confirmationNumber, receipt) {
        console.log('------ confirmation')
        console.log(confirmationNumber)
        console.log(receipt.status)
    })
    .then(function (newContractInstance) {
        console.log('------ newContractInstance')
        console.log(newContractInstance.options.address) // instance with the new contract address
    });
```

### methods

```
myContract.methods.myMethod([param1[, param2[, ...]]])
```

为该方法创建一个事务对象，然后可以调用，发送，估计。

这种智能合约的方法可通过以下方式获得：

```
名字： myContract.methods.myMethod(123)
带参数的名称： myContract.methods['myMethod(uint256)'](123)
签名： myContract.methods['0x58cf5f10'](123)
```

这允许调用与 JavaScript 合同对象具有相同名称但不同参数的函数。

**返回**

Object：事务对象：

-   Array - arguments：之前传递给方法的参数。他们可以改变。
-   Function- call：将调用“常量”方法并在 EVM 中执行其智能合约方法而不发送交易（不能改变智能合约状态）。
-   Function- send：将事务发送到智能合约并执行其方法（可以改变智能合约状态）。
-   Function- estimateGas：将估算方法在链上执行时使用的气体。
-   Function- encodeABI：为此方法编码 ABI。这可以使用事务发送，调用方法或传入另一个智能合约方法作为参数。

### methods.myMethod.call

```
myContract.methods.testCall1().call()
    .then(data => {
        console.log('testCall1 data', data)
    })
    .catch(function (error) {
        console.log('estimateGas error', error)
    });
```

### methods.myMethod.send

### methods.myMethod.estimateGas

```
myContract.methods.testSend2(200, 201)
    .estimateGas({ from: "0x4b983d2cb24ac3953aa2ae1a0ceba4e5f1e1a5da", gas: 8 })
    .then(data => {
        console.log('estimateGas data', data)
    }).catch(function (error) {
        console.log('estimateGas error', error)
    });
```

背后调用的 method

```
{
	"jsonrpc":"2.0",
	"id":5,
	"method":"eth_estimateGas",
	"params":[
		{
			"data":"0x125d9d9000000000000000000000000000000000000000000000000000000000000000c800000000000000000000000000000000000000000000000000000000000000c9",
			"to":"0x4B983D2cb24AC3953AA2Ae1A0CeBa4e5F1e1A5da",
			"from": "0x4B983D2cb24AC3953AA2Ae1A0CeBa4e5F1e1A5da",
			"gas": "0x0"
		}
	]

}
```

### methods.myMethod.encodeABI

```
let encodeABIData = myContract.methods.testSend1().encodeABI();
console.log(encodeABIData)
```

# 事件

-   once
-   events
-   events.allEvents
-   getPastEvents

以后再实现

### once

```
myContract.once(event[, options], callback)
```

订阅事件并在第一个事件或错误后立即取消订阅。只会为一个事件执行。

### events

```
myContract.events.MyEvent([options][, callback])
```

订阅事件

### events.allEvents

```
myContract.events.allEvents([options][, callback])
```

与事件相同，但接收来自此智能合约的所有事件。可选地，filter 属性可以过滤这些事件。

### getPastEvents

```
myContract.getPastEvents(event[, options][, callback])
```

获取此合约的过去事件。
