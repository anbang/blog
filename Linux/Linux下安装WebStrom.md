### 首先下载文件

首先需要下载WebStrom的Linux安装包,地址如下: 

`https://www.jetbrains.com/webstorm/download/`

然后运行如下命令解压并安装并运行WebStorm:


1.解压缩

```bash
sudo tar xfz ~/Downloads/WebStorm-16.2.3.tar.gz
```

2.解压文件移动到/opt下
```bash
sudo mv ~/Downloads/WebStorm-162.2228.20 /opt/
```

3.进入到此文件夹中:
````bash
cd /opt/WebStorm-162.2228.20/bin/
````

4.启动webstorm:
```bash
sudo sh webstorm.sh
```