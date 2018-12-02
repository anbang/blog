开发Electron应用的时候，遇到一个错误；

```
(anonymous) @ G:\CanonChain\canonchain-wallet\node_modules\electron\dist\resources\electron.asar\renderer\init.js:162
G:\CanonChain\canonchain-wallet\node_modules\bindings\bindings.js:88 

Uncaught Error: A dynamic link library (DLL) initialization routine failed.

\\?\G:\CanonChain\canonchain-wallet\node_modules\argon2\build\Release\argon2.node
    at process.module.(anonymous function) [as dlopen] (ELECTRON_ASAR.js:172:20)
    at Object.Module._extensions..node (module.js:598:18)
    at Object.module.(anonymous function) [as .node] (ELECTRON_ASAR.js:172:20)
    at Module.load (module.js:503:32)
    at tryModuleLoad (module.js:466:12)
    at Function.Module._load (module.js:458:3)
    at Module.require (module.js:513:17)
    at require (internal/module.js:11:18)
    at bindings (G:\CanonChain\canonchain-wallet\node_modules\bindings\bindings.js:81:44)
    at Object.<anonymous> (G:\CanonChain\canonchain-wallet\node_modules\argon2\argon2.js:3:37)
```
 

问题搜索；

https://stackoverflow.com/questions/36029955/electron-uncaught-error-a-dynamic-link-library-dll-initialization-routine-fai

解决方案：



https://github.com/electron/electron/blob/v0.37.2/docs/tutorial/using-native-node-modules.md#using-native-node-modules

或者查看

https://github.com/octalmage/robotjs/wiki/Electron

Node ABI查看 ：https://nodejs.org/zh-cn/download/releases/  其中`NODE_MODULE_VERSION`就是abi；

比如 `Node.js 8.4.0` 是 `57`

## 我收先操作了

```
npm install electron-rebuild
```

参考来源：https://github.com/JetBrains/teamcity-vscode-extension/issues/11

## 然后遇到了新的问题

```
Uncaught Error: Could not locate the bindings file. Tried:
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\build\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\build\Debug\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\build\Release\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\out\Debug\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\Debug\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\out\Release\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\Release\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\build\default\argon2.node
 → G:\CanonChain\canonchain-wallet\node_modules\argon2\compiled\8.2.1\win32\x64\argon2.node
    at bindings (G:\CanonChain\canonchain-wallet\node_modules\bindings\bindings.js:93)
    at Object.<anonymous> (G:\CanonChain\canonchain-wallet\node_modules\argon2\argon2.js:3)
    at Object.<anonymous> (G:\CanonChain\canonchain-wallet\node_modules\argon2\argon2.js:125)
    at Module._compile (module.js:569)
    at Object.Module._extensions..js (module.js:580)
    at Module.load (module.js:503)
    at tryModuleLoad (module.js:466)
    at Function.Module._load (module.js:458)
    at Module.require (module.js:513)
    at require (internal/module.js:11)
```

参考来源 ： https://github.com/libxmljs/libxmljs/issues/253

我做了如下操作
```
rm -rf node_modules/
npm install bindings
```

但是，又会到第一个错误了；
