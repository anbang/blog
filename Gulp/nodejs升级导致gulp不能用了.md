在写node多页应用的时候，升级了nodejs；

过了会，回到另外一个项目，需要用gulp的时候，任务列表挂了；

![](./img/gulp-error-01.png)

```
at Object.<anonymous> (F:\xxxxxxx\node_modules\gulp-scp2\node_modules\scp2\node_modules\glob\node_modules\graceful-fs\fs.js:11:1)
```

![](./img/gulp-error-02.png)

这个问题是升级了nodejs后导致的；

相关依赖报错，解决方法是升级相关依赖  graceful-fs 的版本：

可以看下当前的依赖；
```
npm list graceful-fs 
```

解决办法：就删了node_modules重装，记得清除npm的缓存；
```
npm cache clean -f
```
删除后再install一次；

![](./img/gulp-error-03.png) 

再不行就慢慢查是哪个依赖的问题了，或者依赖都给升级了；

如果再不行，应该就是回滚NODE版本了把，6.0以及以后的版本，gulp有些不能用，另外建议,


```
nodejs使用稳定版本，
nodejs使用稳定版本，
nodejs使用稳定版本；
```

如果还不行，