build Electron项目时候遇到的错误

```
 OKAY  take it away `electron-builder`

  • electron-builder version=19.56.2
  • loaded configuration file=package.json ("build" field)
  • writing effective config file=build/electron-builder.yaml
  • rebuilding native production dependencies platform=darwin arch=x64
Error: /usr/local/bin/node exited with code 1
Output:

> argon2@0.19.3 install /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/argon2
> node-gyp rebuild

  CC(target) Release/obj.target/libargon2/argon2/src/opt.o
  CC(target) Release/obj.target/libargon2/argon2/src/argon2.o
  CC(target) Release/obj.target/libargon2/argon2/src/core.o
  CC(target) Release/obj.target/libargon2/argon2/src/blake2/blake2b.o
  CC(target) Release/obj.target/libargon2/argon2/src/thread.o
  CC(target) Release/obj.target/libargon2/argon2/src/encoding.o
  LIBTOOL-STATIC Release/argon2.a
  CXX(target) Release/obj.target/argon2/src/argon2_node.o
  SOLINK_MODULE(target) Release/argon2.node

> ed25519@0.0.4 install /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519
> node-gyp rebuild

  CC(target) Release/obj.target/ed25519/src/ed25519/keypair.o

Error output:
../src/ed25519/keypair.c:2:10: fatal error: 'openssl/sha.h' file not found
#include <openssl/sha.h>
         ^~~~~~~~~~~~~~~
1 error generated.
make: *** [Release/obj.target/ed25519/src/ed25519/keypair.o] Error 1
gyp ERR! build error 
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:258:23)
gyp ERR! stack     at emitTwo (events.js:125:13)
gyp ERR! stack     at ChildProcess.emit (events.js:213:7)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:200:12)
gyp ERR! System Darwin 17.3.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/ed25519
gyp ERR! node -v v8.4.0
gyp ERR! node-gyp -v v3.6.2
gyp ERR! not ok 
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! ed25519@0.0.4 install: `node-gyp rebuild`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the ed25519@0.0.4 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/broszhu/.npm/_logs/2018-09-29T02_31_39_815Z-debug.log

    at ChildProcess.childProcess.once.code (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/builder-util/src/util.ts:241:14)
    at Object.onceWrapper (events.js:318:30)
    at emitTwo (events.js:125:13)
    at ChildProcess.emit (events.js:213:7)
    at maybeClose (internal/child_process.js:927:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:211:5)
From previous event:
    at spawn (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/builder-util/src/util.ts:200:3)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/src/util/yarn.ts:171:5
From previous event:
    at rebuild (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/out/util/yarn.js:93:22)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/src/util/yarn.ts:20:11
    at Generator.next (<anonymous>)
    at runCallback (timers.js:781:20)
    at tryOnImmediate (timers.js:743:5)
    at processImmediate [as _immediateCallback] (timers.js:714:5)
From previous event:
    at installOrRebuild (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/out/util/yarn.js:31:21)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/src/packager.ts:442:7
    at Generator.next (<anonymous>)
From previous event:
    at Packager.installAppDependencies (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/out/packager.js:496:11)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/src/packager.ts:356:20
    at Generator.next (<anonymous>)
From previous event:
    at Packager.doBuild (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/out/packager.js:432:11)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/src/packager.ts:308:52
    at Generator.next (<anonymous>)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/graceful-fs/graceful-fs.js:99:16
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/graceful-fs/graceful-fs.js:43:10
    at FSReqWrap.oncomplete (fs.js:135:15)
From previous event:
    at Packager._build (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/out/packager.js:376:11)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/src/packager.ts:270:23
    at Generator.next (<anonymous>)
    at runCallback (timers.js:781:20)
    at tryOnImmediate (timers.js:743:5)
    at processImmediate [as _immediateCallback] (timers.js:714:5)
From previous event:
    at Packager.build (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder-lib/out/packager.js:332:11)
    at /Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/src/builder.ts:310:40
    at Generator.next (<anonymous>)
From previous event:
    at _build (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/out/builder.js:61:21)
    at build (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/src/builder.ts:280:10)
    at then (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/src/cli/cli.ts:48:33)
    at runCallback (timers.js:781:20)
    at tryOnImmediate (timers.js:743:5)
    at processImmediate [as _immediateCallback] (timers.js:714:5)
From previous event:
    at Object.args [as handler] (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/out/cli/cli.js:121:117)
    at Object.runCommand (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/node_modules/yargs/lib/command.js:235:44)
    at Object.parseArgs [as _parseArgs] (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/node_modules/yargs/yargs.js:1042:24)
    at Object.get [as argv] (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/node_modules/yargs/yargs.js:957:21)
    at Object.<anonymous> (/Users/broszhu/Documents/wifisong-test/canonchain-wallet/node_modules/electron-builder/out/cli/cli.js:117:439)
    at Module._compile (module.js:573:30)
    at Object.Module._extensions..js (module.js:584:10)
    at Module.load (module.js:507:32)
    at tryModuleLoad (module.js:470:12)
    at Function.Module._load (module.js:462:3)
    at Function.Module.runMain (module.js:609:10)
    at startup (bootstrap_node.js:158:16)
    at bootstrap_node.js:598:3
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! czr-wallet@0.1.2 build: `node .electron-vue/build.js && electron-builder`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the czr-wallet@0.1.2 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/broszhu/.npm/_logs/2018-09-29T02_31_39_903Z-debug.log

```

核心错误：
```
Error output:
../src/ed25519/keypair.c:2:10: fatal error: 'openssl/sha.h' file not found
#include <openssl/sha.h>
```

需要下面这样操作

```
$ cd /usr/local/include 
$ ln -s ../opt/openssl/include/openssl .
```
