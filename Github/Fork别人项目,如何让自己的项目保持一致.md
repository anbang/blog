如何Fork别人的代码:

### fork别人的代码，并pull request

- 1.注册自己的账号
- 2.fork别人的的项目
- 3.克隆自己Fork的项目到本地
- 4.修改文件
    - git add –A
    - git commit -m”提交到历史区”
    - git push origin master 提交到 github上
- 5.发起一个pull request
- 6.别人合并代码


##### 可能出现的问题

- 1)如果是一个空的文件夹或者是一个空的文件是上传不上去的
- 2)如果出现这两个信息说明，当前本地的git还没有注册
```
git config --global user.email "...."
git config --global user.name "...."
```

解决办法:
```
$ git config --global user.email "你的邮箱(建议和你们的github注册邮箱保持一致)"
$ git config --global user.name "你的名字(建议和github用户名保持一致)" 
```

### 让自己Fork的github项目，与源项目保持一致
- 添加源项目的文件[teacher是随便写的变量名]
- 拉取到本地，如果是自己的项目，可以用`git pull origin master`

```bash
git remote add teacher https://github.com/zhubangbang/blog.git
git pull teacher master
```
