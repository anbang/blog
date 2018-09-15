**场景** ：发布Npm包时候，报错了；

执行`npm publish`,

开始报错了

```
npm ERR! publish Failed PUT 401
npm ERR! code E401
npm ERR! 404 unauthorized Login first: zhubangbang
npm ERR! 404
npm ERR! 404  ‘zhubangbang’ is not in the npm registry.
npm ERR! 404 You should bug the author to publish it (or use the name yourself!)
npm ERR! 404
npm ERR! 404 Note that you can also install from a
npm ERR! 404 tarball, folder, http url, or git url.
npm ERR! A complete log of this run can be found in:
```

显示账号没有注册，其实是因为npm以前切到淘宝源的问题；

解决：通过安装nrm

```
npm install -g nrm
```

从淘宝源切到npm源；

再次执行发布；报错了

```
npm ERR! code ENEEDAUTH
npm ERR! need auth auth required for publishing
npm ERR! need auth You need to authorize this machine using `npm adduser`
```

执行`npm adduser`

```
Username: zhubangbang
Password:
Email: (this IS public) 79****07@qq.com
Logged in as zhubangbang on https://registry.npmjs.org/.
```
再次知性
```
npm publish
```
这样就发布成功了，

发布成功后，会收到npm官方给的一个邮件

