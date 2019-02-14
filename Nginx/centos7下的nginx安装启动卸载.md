# Centos下使用yum安装nginx

1、添加源

默认情况Centos7中无Nginx的源，Nginx官网提供了Centos的源地址。因此可以如下执行命令添加源：

`sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`

2、安装Nginx

通过 `yum search nginx` 看看是否已经添加源成功。

如果成功则执行下列命令安装Nginx。

`sudo yum install -y nginx`
 

# 启动Nginx并设置开机自动运行

### 启动Nginx
```
sudo systemctl start nginx
```

### 开机自动运行
```
sudo systemctl enable nginx
```

### 浏览查看效果

在浏览器中输入您的服务器地址，注意需要打开80端口；nginx默认是监听80端口的

如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：(如果防火墙关了,可直接跳过)

设置防火墙

允许http通信

`sudo firewall-cmd --permanent --zone=public --add-service=http`

允许https通信

`sudo firewall-cmd --permanent --zone=public --add-service=https`

重启防火墙

`sudo firewall-cmd --reload`

# Nginx卸载

### 停止Nginx软件

`service nginx stop`

### 删除Nginx的自动启动

`chkconfig nginx off`

### 从源头删除Nginx

```
rm -rf /usr/sbin/nginx
rm -rf /etc/nginx
rm -rf /etc/init.d/nginx
```

### 再使用yum清理

`yum remove nginx`