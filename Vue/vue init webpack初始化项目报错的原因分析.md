安装vue-cli后，

第一步就是初始化项目，这步的操作正常是没有问题的；

但是如果遇到了问题；

可能是因为下面的原因导致

1、nodejs的版本太老了；

`解决办法：升级当前的node版本`

参考：https://zhubangbang.com/upgraded-nodejs-to-the-latest-version.html

2、端口冲突，

`解决办法：修改端口`

一般是这2个原因导致的；

正常的安装，就是一路按照提示操作即可；

``` 
Project name (vue-test) 直接回车默认

Project description (A Vue.js project) 直接回车默认

Author 直接回车默认

Use ESLint to lint your code? n（语法检测，个人是不需要）

pick an eslint preset. 默认Standard

setup unit tests with karma + mocha?No(单元测试不需要)

setup e2e tests with Nightwatch?No(单元测试不需要)

测试有没有正常初始化；执行run即可

npm run dev
```

则会自动打开一个界面，如果报错不能打开网页的话只有一种原因，那就端口占用，这个时需要到/config/index.js目录下改端口就行。

目前一般是这两个原因，具体啥原因，还是要看报错信息的  = =！