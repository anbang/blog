gulp是自动化执行工具；一些任务需要重复的执行，都可以用gulp来完成；

- 把文件从开发目录拷贝到生产目录
- 把多个 JS 或者 CSS 文件合并成一个文件
- 对JS文件和CSS进行压缩
- 把sass或者less文件编译成CSS
- 压缩图像文件
- 创建一个可以实时刷新页面内容的本地服务器

gulp用的是node中流的概念，上一级的输出是下一级的输入；流是一种有起点和终点的数据传输手段；

安装gulp：基于node的，先要安装node环境；

```
npm init //初始化项目
npm install gulp –-save-dev //在node_modules下安装本地的gulp库；并且把配置写到package.json里
```

#### 注意：var gulp = require(‘gulp’);

gulp的任务必须放在gulpfile.js里，需要新建一个gulpfile.js的文件,文件名不可改变

```javascript
var gulp = require(‘gulp’);
//通过require可以把gulp模块引入当前项目并赋值给gulp变量，这样gulp这个变量里面就会拥有gulp这个变量里的所有gulp的方法;
```

gulp的工作方式;

gulp的使用流程一般是：

```
1、首先通过gulp.src()方法获取到想要处理的文件流
2、把文件流通过pipe方法导入到gulp插件中，
3、最后把经过插件处理后的流再通过pipe()方法导入到gulp.dest()中
```

gulp的4个核心API：

``` 
gulp.src()
gulp.dest()
gulp.task()
gulp.watch()
```
演示代码：

```javascript
gulp.task('test', function () {
gulp.src('client/templates/*.jade')
        .pipe(jade())
        .pipe(minify())
        .pipe(gulp.dest('build/minified_templates'));
});
```


1、gulp.src():获取数据流；

gulp是图stream为媒介，不需要频繁的生成临时文件，所以速度会很快；需要注意的是gulp.src()这个流的内容不是原始的文件流，而是一个虚拟文件对象流，这个虚拟文件对象中存储着原始文件的路径，文件名和内容等信息；


## 需要熟知的参数标记，其他所有的参数标记只在一些任务需要的时候使用。

- v 或 –version 会显示全局和项目本地所安装的 gulp 版本号
- gulpfile 手动指定一个 gulpfile 的路径，这在你有很多个 gulpfile 的时候很有用。这也会将 CWD 设置到该 gulpfile 所在目录
- cwd dirpath 手动指定 CWD。定义 gulpfile 查找的位置，此外，所有的相应的依赖（require）会从这里开始计算相对路径
- T 或 –tasks 会显示所指定 gulpfile 的 task 依赖树
- tasks-simple 会以纯文本的方式显示所载入的 gulpfile 中的 task 列表
- color 强制 gulp 和 gulp 插件显示颜色，即便没有颜色支持
- no-color 强制不显示颜色，即便检测到有颜色支持
- silent 禁止所有的 gulp 日志


