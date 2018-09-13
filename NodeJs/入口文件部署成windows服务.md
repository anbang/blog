**场景**:在Windows服务器上进行部署，默认是一个终端进行跑，但是容易被别人误操作给关掉；

**解决**:做成Windwos服务，这样除非刻意的操作，否则都不会有问题的了；

**优化**:避免意外

- 1-做开机启动，设置该服务为开机自启动，即使服务器重启也没事；
- 2-做服务守护，假设范围是`ZAB`打头的服务，把服务的名字设置为`ZAB-XXX`这样就进行批量守护了

**强行说的理由**：部署nodejs的项目，大家都会用到forever这个库，这个库相当好用，可以让nodejs的站点在后台跑，不需要cmd的窗口一直开着。在windows下，如果用户一直不注销，这种方式是可行的，但在服务器上的话就麻烦了，因为服务器在部署完成后，一般都会注销，那么站点就挂了。（反正就是我想弄成widows服务，上面都是扯的）

# 步骤

- 1、安装需要的包
- 2、写安装服务
- 3、写卸载服务
- 4、执行安装或者卸载服务

### 1、首先全局安装全局安装node-windows包

```
npm install -g node-windows
```

### 2、安装服务的书写(install.js)

```javascript
let Service = require('node-windows').Service;
let svc = new Service({
    name: 'wifisong.canonchain.explorer',    //服务名称
    description: 'CanonChain的浏览器服务', //描述
    script: 'E:/canonchain-explorer/explorer-backend/bin/www' //nodejs项目要启动的文件路径
});
svc.on('install', () => {
    console.log("Install Success")
    svc.start();
});
svc.install();
```

### 3、卸载服务的书写(uninstall.js)

```javascript
let Service = require('node-windows').Service;
let svc = new Service({
    name: 'wifisong.canonchain.explorer',    //服务名称
    description: 'CanonChain的浏览器服务', //描述
    script: 'E:/canonchain-explorer/explorer-backend/bin/www' //nodejs项目要启动的文件路径
});
svc.on('uninstall', function () {
    console.log('Uninstall Complete.');
    console.log('The service exists: ', svc.exists);
});
svc.uninstall();
```

4、执行安装/卸载

```
node install.js //安装服务
node uninstall //卸载服务
```



