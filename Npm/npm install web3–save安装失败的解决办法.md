在window的时候安装web3.js失败了；

报错信息如下，

```
    Administrator@DDEX7BK7XCS2QTJ MINGW32 /e/local/web3-eth
    $ npm install web3 –save
    npm WARN deprecated fs-promise@2.0.3: Use mz or fs-extra^3.0 with Promise Suppor                                                  t
    npm WARN deprecated minimatch@0.3.0: Please update to minimatch 3.0.2 or higher                                                   to avoid a RegExp DoS issue

    > scrypt@6.0.3 preinstall E:\local\web3-eth\node_modules\scrypt
    > node node-scrypt-preinstall.js

    > websocket@1.0.24 install E:\local\web3-eth\node_modules\websocket
    > (node-gyp rebuild 2> builderror.log) || (exit 0)

    E:\local\web3-eth\node_modules\websocket>if not defined npm_config_node_gyp (nod                                                  e “C:\Users\Administrator\AppData\Roaming\npm\node_modules\npm\bin\node-gyp-bin\                                                  \..\..\node_modules\node-gyp\bin\node-gyp.js” rebuild )  else (node “” rebuild )                                                 
    ▒ڴ˽▒▒▒▒▒▒▒▒һ▒▒▒▒▒▒һ▒▒▒▒Ŀ▒▒▒▒Ҫ▒▒▒ò▒▒▒▒▒▒ɣ▒▒▒▒▒ӡ▒/m▒▒▒▒▒ء▒
    C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140\Platforms\x64\PlatformToo                                                  lsets\v140\Toolset.targets(36,5): error MSB8036: The Windows SDK version 8.1 was                                                   not found. Install the required version of Windows SDK or change the SDK versio                                                  n in the project property pages or by right-clicking the solution and selecting                                                   “Retarget solution”. [E:\local\web3-eth\node_modules\websocket\build\bufferutil.                                                  vcxproj]
    C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140\Platforms\x64\PlatformToo                                                  lsets\v140\Toolset.targets(36,5): error MSB8036: The Windows SDK version 8.1 was                                                   not found. Install the required version of Windows SDK or change the SDK versio                                                  n in the project property pages or by right-clicking the solution and selecting                                                   “Retarget solution”. [E:\local\web3-eth\node_modules\websocket\build\validation.                                                  vcxproj]

    > sha3@1.2.0 install E:\local\web3-eth\node_modules\sha3
    > node-gyp rebuild

    E:\local\web3-eth\node_modules\sha3>if not defined npm_config_node_gyp (node “C:                                                  \Users\Administrator\AppData\Roaming\npm\node_modules\npm\bin\node-gyp-bin\\..\.                                                  .\node_modules\node-gyp\bin\node-gyp.js” rebuild )  else (node “” rebuild )
    ▒ڴ˽▒▒▒▒▒▒▒▒һ▒▒▒▒▒▒һ▒▒▒▒Ŀ▒▒▒▒Ҫ▒▒▒ò▒▒▒▒▒▒ɣ▒▒▒▒▒ӡ▒/m▒▒▒▒▒ء▒
    C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140\Platforms\x64\PlatformToo                                                  lsets\v140\Toolset.targets(36,5): error MSB8036: The Windows SDK version 8.1 was                                                   not found. Install the required version of Windows SDK or change the SDK versio                                                  n in the project property pages or by right-clicking the solution and selecting                                                   “Retarget solution”. [E:\local\web3-eth\node_modules\sha3\build\sha3.vcxproj]
    gyp ERR! build error
    gyp ERR! stack Error: `C:\Program Files (x86)\MSBuild\14.0\bin\msbuild.exe` fail                                                  ed with exit code: 1
    gyp ERR! stack     at ChildProcess.onExit (C:\Users\Administrator\AppData\Roamin                                                  g\npm\node_modules\npm\node_modules\node-gyp\lib\build.js:258:23)
    gyp ERR! stack     at emitTwo (events.js:125:13)
    gyp ERR! stack     at ChildProcess.emit (events.js:213:7)
    gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_proces                                                  s.js:200:12)
    gyp ERR! System Windows_NT 6.1.7601
    gyp ERR! command “C:\\Program Files\\nodejs\\node.exe” “C:\\Users\\Administrator                                                  \\AppData\\Roaming\\npm\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gy                                                  p.js” “rebuild”
    gyp ERR! cwd E:\local\web3-eth\node_modules\sha3
    gyp ERR! node -v v8.6.0
    gyp ERR! node-gyp -v v3.6.2
    gyp ERR! not ok
    npm WARN web3-eth@1.0.0 No description
    npm WARN web3-eth@1.0.0 No repository field.

    npm ERR! code ELIFECYCLE
    npm ERR! errno 1
    npm ERR! sha3@1.2.0 install: `node-gyp rebuild`
    npm ERR! Exit status 1
    npm ERR!
    npm ERR! Failed at the sha3@1.2.0 install script.
    npm ERR! This is probably not a problem with npm. There is likely additional log                                                  ging output above.

    npm ERR! A complete log of this run can be found in:
    npm ERR!     C:\Users\Administrator\AppData\Roaming\npm-cache\_logs\2017-10-18T0                                                  1_58_46_807Z-debug.log

```

我的规避方法是直接安装对应版本号的web3.js;
```
npm install web3@^0.20.0
```
这时候安装成功了。。