ubuntu的方法很多，apt里面非常旧里，还是4.x的;

源码解压编译其实并不省心，

我是这么安装

```bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

如果安装nodejs 9.x版本

```bash
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
sudo apt-get install -y nodejs
```

感觉这种方法还是非常ok;