Electron开发客户端时候，碰到一个错误

错误信息如下
```
/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/bindings/bindings.js:96 

Uncaught Error: Could not locate the bindings file. Tried:
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/build/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/build/Debug/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/build/Release/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/out/Debug/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/Debug/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/out/Release/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/Release/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/build/default/ed25519.node
 → /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/compiled/8.2.1/darwin/x64/ed25519.node
    at bindings (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/bindings/bindings.js:93:9)
    at Object.<anonymous> (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/index.js:1:192)
    at Object.<anonymous> (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519/index.js:3:3)
    at Module._compile (module.js:569:30)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:503:32)
    at tryModuleLoad (module.js:466:12)
    at Function.Module._load (module.js:458:3)
    at Module.require (module.js:513:17)
    at require (internal/module.js:11:18)
```

解决办法是重新构建包

使用新的二进制文件重新编译所有 C++ 插件。

当前项目下

```
npm rebuild
```

再次启动就可以了；

参考issues https://github.com/stephen/airsonos/issues/357

