开发客户端一定要做的就是自动更新模块，否则每次版本升级都是一个头疼的事。

1.安装 electron-updater 包模块
```
npm install electron-updater --save
```

2.配置package.json文件
2.1 为了打包时生成latest.yml文件，需要在 build 参数中添加 publish 配置。

> 注意：配置了publish才会生成latest.yml文件，用于自动更新的配置信息；latest.yml文件是打包过程生成的文件，为避免自动更新出错，打包后禁止对latest.yml文件做任何修改。如果文件有误，必须重新打包获取新的latest.yml文件！！！


nsis配置不会影响自动更新功能，但是可以优化用户体验，比如是否允许用户自定义安装位置、是否添加桌面快捷方式、安装完成是否立即启动、配置安装图标等。nsis 配置也是添加在 build 参数中。 详细参数配置可参见官方文档 nsis配置。

https://www.electron.build/configuration/nsis

```
  "build": {
    "appId": "com.canonchain.pekka",
    "productName": "pekka",
    "artifactName": "${productName}-${version}.${ext}",
    "directories": {
      "output": "dist"
    },
    "publish": [
      {
        "provider": "generic",
        "url": "https://canonchain-public.oss-cn-hangzhou.aliyuncs.com/pekka/"
      }
    ],
    "files": [
      "**/*",
      "!screenshot/${/*}",
      "!dist/win-unpacked/d3dcompiler_47.dll",
      "!docs${/*}"
    ],
    "win": {
      "icon": "build/icon.ico",
      "target": [
        {
          "target": "nsis"
        }
      ]
    },
    "nsis": {
      "oneClick": false,
      "allowElevation": true,
      "allowToChangeInstallationDirectory": true,
      "installerIcon": "./build/icon.ico",
      "uninstallerIcon": "./build/icon.ico",
      "installerHeaderIcon": "./build/icon.ico",
      "createDesktopShortcut": true,
      "createStartMenuShortcut": true,
      "shortcutName": "PEKKA"
    },
    "copyright": "Copyright © PEKKA"
  }
```


3.配置主进程main.js文件（或主进程main中的index.js文件），引入 electron-updater 文件，添加自动更新检测和事件监听：
注意：一定要是主进程main.js文件（或主进程main中的index.js文件），否则会报错。
注意：这个autoUpdater不是electron中的autoUpdater，是electron-updater的autoUpdater！


```
    //检测更新
    updateHandle();
    if (process.env.NODE_ENV !== 'dev') {
        autoUpdater.checkForUpdates()
    }
```

```
// 检测更新，在你想要检查更新的时候执行，renderer事件触发后的操作自行编写
function updateHandle() {
    let message = {
        error: '检查更新出错',
        checking: '正在检查更新……',
        updateAva: '检测到新版本，正在下载……',
        updateNotAva: '现在使用的就是最新版本，不用更新',
    };

    autoUpdater.on('error', (error) => {
        mainLogs.error(message.error);
        mainLogs.error(error == null ? "unknown" : (error.stack || error).toString())
    })

    autoUpdater.on('checking-for-update', () => {
        mainLogs.info(message.checking);
    })

    autoUpdater.on('update-available', info => {
        mainLogs.info(`${message.updateAva} ${JSON.stringify(info)}`);
    })

    autoUpdater.on('update-not-available', info => {
        mainLogs.info(`${message.updateNotAva}`);
    })

    autoUpdater.on('download-progress', ({ delta, bytesPerSecond, percent, total, transferred }) => {
        mainLogs.info(`更新下载中...delta: ${delta}，bytesPerSecond: ${bytesPerSecond}，percent: ${percent}，total: ${total}，transferred: ${transferred}`)
    })

    autoUpdater.on('update-downloaded', info => {
        mainLogs.info(`开始更新钱包程序 info:${JSON.stringify(info)}`);
        setImmediate(() => autoUpdater.quitAndInstall())
    })
}
```

参考地址：https://segmentfault.com/a/1190000012904543