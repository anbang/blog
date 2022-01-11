Mist 是以太坊官方提供的浏览器，通过Mist我们可以很方便的连接上我们的私有网络，从而更好的开发、调试、测试我们的智能合约。

Github地址：
```
https://github.com/ethereum/mist 类浏览器 
https://github.com/ethereum/meteor-dapp-wallet 类界面 
Mist的Api：https://github.com/ethereum/mist/blob/develop/MISTAPI.md 
常见问题：https://github.com/ethereum/mist/wiki
```

项目准备/依赖：

mist项目需要以下的环境：

```
nodejs 
gulp 
electron 
meteor
yarn  //类似npm的包管理工具
```

准备开发：

```
$ git clone https://github.com/ethereum/mist.git
$ cd mist
$ yarn
```

yarn的时候，遇到了一个错误；

image

```
error MSB8036:  The Windows SDK version 8.1 was not found. Install the required version of Windows SDK or change the SDK version in the project property pages or by right-clicking the solution and selecting “Retarget solution”.
```

windows上每次搞node都非常蛋疼，妹的，一会配置这，一会配置那的，为什么就不能像linux/mac那样优雅呢！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！

尝试的解决办法：

搜到用`node-gyp` ,编码的原因；

https://github.com/nodejs/node-gyp#installation

### 解决1：

```
    1、安装windows构建工具
    2、其它工具和配置
        安装Visual Studio 2015（或修改现有的安装） 选择 Common Tools for Visual C++
        如果是windows7 还需要.NET Framework 4.5.1
        安装Python 2.7（不支持v3.x.x，如果是多版本需要默认2.7）

```
### 解决2 

管理员权限运行下面命令

```
npm install --global --production windows-build-tools
```

管理员cmd在 `C:\Windows\System32`

运行`Mist`：