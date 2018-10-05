使用命令安装  
```
apt install manpages-zh -y
```

使用 `man -M /usr/share/man/zh_CN command` 的形式来使用中文man。

比如：

`man -M /usr/share/man/zh_CN ls`

但是一直这么用，也是蛋疼的；

可以设置`manpath.config`配置文件来使用默认的`zh_CN`.

修改man默认的语言
```
sudo gedit /etc/manpath.config
```
把里面的所有的`/usr/share/man`改成`/usr/share/man/zh_CN`