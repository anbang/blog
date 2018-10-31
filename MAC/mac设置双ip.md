使用命令：

```
sudo ifconfig en0 inet 192.168.10.197 netmask 255.255.255.0 alias
```

说明：
- inet后面跟的是你想要设置的ip地址
- netmask后面跟的是子网掩码
- alias，设置命令的别名

设置后输入ifconfig可以查看设置后的效果;

删除使用 
```
sudo ifconfig en0 -alias 10.0.6.197
```


### 第二种

最好的解决办法是能够给Mac分配一个固定的IP，不管怎么切换网络一直存在。幸好这个是可以解决的，而且并不复杂。方法如下：

ifconfig lo0 alias 192.168.255.199
其原理是给网络设备loopback增加一个alias。loopback设备一直存在，而且与en0设备独立，可以一直存在。

删除方法也很简单：

ifconfig lo0 -alias 192.168.255.199
这样终于可以有个固定IP用来绑定服务了。