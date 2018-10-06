gzip是Linux下最流行的压缩工具

```
gzip        用来压缩文件
gzcat       用来查看压缩过的文本文件的内容
gunzip      用来解压缩文件
```

使用如下
```
gzip sda.js
gunzip sda.js.gz 
```

## tar相关的

语法如下
```
tar function [options] obj1 obj2
```
function定义了tar应该作什么；

```
-c ：       create      建立一个tar归档文件
-x ：       extract     解开一个压缩文件的参数指令！
-t ：       list        查看 tarfile 里面的内容
特别注意，在参数的下达中， c/x/t 仅能存在一个！不可同时存在！ 因为不可能同时压缩与解压缩。 
```
为了配合`c x` 还有命令

```
-v      压缩的过程中显示文件
-f      输出结果到文件！如果用f就一定要把f放在最后
-z      将输出重定向到gzip命令来压缩内容
-j      将输出重定向到bzip2命令来压缩内容
-p      保留所文件权限
```

一般常用`tar -*vf xxx.tar dir/`这样。*可以为`c x`

### 范例一：将整个 `/demo` 目录下的文件全部打包成为 `demo.tar`
```
# tar -cvf demo.tar demo/      　　　　<==仅打包，不压缩！ 
# tar -zcvf demo.tar.gz demo/  　　　　<==打包后，以 gzip 压缩 
# tar -jcvf demo.tar.bz2 demo/　　　　 <==打包后，以 bzip2 压缩 

# 特别注意，在参数 f 之后的文件档名是自己取的，我们习惯上都用 .tar 来作为辨识。 
# 如果加 z 参数，则以 .tar.gz 或 .tgz 来代表 gzip 压缩过的 tar file
# 如果加 j 参数，则以 .tar.bz2 来作为文档名 
```

### 范例二：查阅上述 demo.tar.gz 文件内有哪些文件？
```
tar -tvf    demo.tar
tar -ztvf   demo.tar.gz
```

### 范例三：将 demo.tar.gz 文件解压缩在 /temp 下

```
cd temp/
tar -xvf ../demo.tar
```