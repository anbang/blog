文件的增删改查；制表符的自动补全还是非常不错的；

## 增加

```
touch filename
mkdir dirname
```
如果需要同时创建多个目录和子目录，需要加`-p`参数

```
mkdir -p  AAA/SSSS/DDD
```

#### 复制文件
```
cp test.js  test2.js        文件拷贝到当前目录
cp test.js  test/xx.js      文件拷贝到其他目录
cp -R  test test2           文件夹整个拷贝到其他文件夹
cp *.js test2               在当前文件夹下递归所有.js的文件，都拷贝到test2文件夹下
```

## 移除

默认情况下，`rm` 不移除目录，而`rmdir`只能移除非空的文件夹，所以`rmdir`一般不使用

```
rm -i test2.js              带提示的文件移除操作
rm test2.js                 直接移除，不需要确认
rm -r                       递归地移除目录及它们的内容
rm -ri                      移除并出提示让用户确认，比较保险
rm -rf                      直接移除了
```

## 修改 

修改文件名是使用`移动`来实现的，`mv`

重命名

```
mv test.js newName.js
mv test newDir
```
移动文件
```
mv test2.js newDir/newName.js           
mv test2/ newDir/
```

## 查询

### 查看文件类型

file后面跟想要查看的文件，即可看是什么类型的
```
file xxx
```

### 查看文件内容

##### 查看全部内容
- cat
- more
- less

**cat查看** ： 

```
cat filename.js
cat -n filename.js
```

延续加载是`more`和`less`,  其中more是当前终端查看，less是临时查看，看完后退回终端；
```
more filename.js
less filename.js
```

##### 查看部分内容
```
tail file.js    查看最后10行
tail -n file.js

head file.js    查看开始10行
head -n file.js
```