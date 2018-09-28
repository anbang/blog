可以使用`n`模块来管理node版本；

第一步:安装n模板；
```
npm install -g n
```
我的Mac下有权限限制的设置，所以我需要sudo一下；

```
sudo npm install -g n 
```
第二步：通过n模板来升级nodejs；

```
n stable 
```
这样就可以的；

当然因为我权限的问题，我还是需要sudo来升级的

```
sudo n stable 
```
现在检测一下升级后的版本；

```
node -v & npm -v 
```
已经都是最新版的了

```
v8.4.0
5.3.0
```