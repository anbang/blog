**场景**：项目中的某个文件（比如JB家浏览器的 `.idea`编辑器相关文件)已经被commit了，并push到远程服务器server了，这时发现`.idea`不应该被git管理，同步到团队每个开发人员那里；

这时在`.gitignore`文件里面添加`*.idea`已经不起作用了,因为`.gitignore`只对从来没有commit过的文件起作用。

每次运行本地项目，都会在`.idea`文件夹下生成一堆文件，运行`git status`的时候，依然能看到这些文件

这时候就需要处理了，不是单纯的`.gitignore`就能解决的

### 第一步，为避免冲突需要先同步下远程仓库
```
git pull
```

### 第二步，在本地项目目录下删除追踪状态
```
git rm -r --cached .
```
因为`.gitignore`文件只能作用于 Untracked Files，也就是那些从来没有被 Git 记录过的文件（自添加以后，从未 add 及 commit 过的文件）,这么做就是把所有的文件追踪初始化；

### 第三步，再次add所有文件

这一步之前，需要检查`.gitignore` 是否正确忽略掉目标文件了，如果没问题；

然后输入以下命令，再次将项目中所有文件添加到本地仓库缓存中
```
git add .
```
不要用`git add A`来添加

### 第四步，添加commit，提交到远程库
```
git commit -m "filter new files"
```
```
git push
```

### 如何恢复对某个文件/文件夹的追踪呢

只需要将该文件/文件夹的规则从`.gitignore中删除即可