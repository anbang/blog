### git配置

- 1、全局配置

用户名配置：
`git config –global user.name “Your Name Here”`

邮箱配置：
`git config –global user.email "your_email@youremail.com"`

查看配置：
`git config –list`

- 2、产生rsa密钥对

```bash
ssh-keygen.exe -t rsa
```
一路回车，会在下面路径生成你的公私钥

[linux]:` ~/.ssh/`

``` 
私钥：id_rsa
公钥：id_rsa.pub
```

- 3、将公钥加入到服务端配置中

用自己用户登录gitweb服务，`profile setting`中有`ssh keys`一项，将自己的公钥复制添加进来

- 4、克隆git的代码到当前路径

修改下面ip为git服务器地址
`git clone git@127.0.0.1:master/linux_filter.git`

- 5、创建本地develop分支

查看本地分支：
`git branch –list`

查看远程分支：
`git branch -r`

创建本地分支：
`git branch develop`

删除本地分支
`git branch -d develop`

- 6、切换分支

`git checkout develop` 切换到develop分支

- 7、从远程服务器更新文件到本地

`git  pull`

- 8、添加/删除一个文件

添加一个新文件:
`git add  newfile`

添加修改和新建的文件：
`git add .`

添加修改和删除的文件：
`git add -u`

添加所有修改：
`git add -A`

删除一个文件
`git rm  file`

- 9、提交修改

`git commit -m “message modify”`

注意：此时的提交只是修改了本地缓存，并没有提交到远程服务器

- 10、提交修改到远程服务器

`git push origin master` 此时可以去git web上查看文件更新

