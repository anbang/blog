### ubuntu install vscode

on ubuntu system install ;

u can check the official website first;

[vscode install on website](https://code.visualstudio.com/docs/setup/linux)

u can download file;

then 

```
sudo dpkg -i <file>.deb
sudo apt-get install -f
```
### 出现问题的解决思路；

如果在第一步遇到问题了，
```
sudo dpkg -i <file>.deb
```
可能某个包米有安装；方法如下

##### 1
```
sudo apt-get -f install
```
可以列出当前的情况

```
bang@x:~/下载$ sudo apt-get -f install
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
正在修复依赖关系... 完成
将会同时安装下列软件：
  python-apt
建议安装：
  python-apt-dbg python-apt-doc
下列【新】软件包将被安装：
  python-apt
升级了 0 个软件包，新安装了 1 个软件包，要卸载 0 个软件包，有 180 个软件包未被升级。
有 1 个软件包没有被完全安装或卸载。
需要下载 149 kB 的归档。
解压缩后会消耗 681 kB 的额外空间。
您希望继续执行吗？ [Y/n] y


```

这个时候输入 y 即可自动的解决了；

等执行完以后；

##### 3

再执行

```
sudo apt-get install -f
```