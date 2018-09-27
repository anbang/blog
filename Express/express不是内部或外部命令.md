折腾了N久；
express 用的是老版本的；

```
    开始是用 npm install -g express-generator来安装用的；但是每次用的时候，出现不是可用的命令行；折腾半天没有解决；我就用老版本了
    我用了；npm install -g express@3.5.0 这个版本来操作的；
```

然后发现环境变量多打了一个符号！！！自己吧自己给坑了！！

 ```
D:\node\node_global;C:\Users\朱安邦\AppData\Roaming\npm
```
中间用分号隔开，而且node_global后面不需要加\符号了；在此电脑，高级系统设置里面可以设置好

**在项目目录下 npm install angular 怎么默认安装到全局了？**

原理是这样的
当使用npm set global true的时候，修改的是本用户配置。
当使用npm set global false的时候，修改的是系统配置。
而起作用的始终是本用户的配置。所以改为false不生效。

所以要改为true的时候需要手工修改本用户的配置文件
```
npm config ls
```
里的`userconfig C:\Users\Administrator\.npmrc`
    
```
npm config ls
```

可以看所有的配置
