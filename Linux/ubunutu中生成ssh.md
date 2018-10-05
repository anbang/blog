### 产生密钥对

```
ssh–keygen.exe -t rsa
```
linux下是下面这种的
```
 ssh-keygen -t rsa -C '79228807@qq.com'
```


 一路回车会在下面产生密钥对；

此时，你的C:\Users\admin\.ssh这个路径下会生成两个文件：id_rsa和id_rsa.pub

用记事本打开id_rsa.pub文件，复制内容，在github.com的网站上到ssh密钥管理页面，添加新公钥，随便取个名字，内容粘贴刚才复制的内容。(`ssh-keygen -t rsa -C "email@email.com"`  也可以后面加github邮箱)

```
window: C:\users\XXX\.ssh
linux: ~/.ssh/
```

将公钥加入服务端配置中；
github — profile –setting –ssh keys –使用公钥；