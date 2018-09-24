上传单文件的方法：
功能：把js/文件下的文件复制到服务器/d:/zhuanbang；
 
```javascript
var gulp = require('gulp');
var scp = require('gulp-scp2');
 
gulp.task('default', function () {
    gulp.src('js/*')
        .pipe(scp({
            host: '****，****，***，***',
            username: 'administrator',
            password: '*******',
            dest: '/d:/zhuanbang'
        }))
        .on('error', function(err) {
            console.log(err);
        });
});
```

命令行输入gulp就可以直接上传过去了；

可以吧js下面所有的文件全部上传过去；

（如果多个命令，可以default换成所需要的名字，比如up，命令行输入的时候写gulp up 就可以了）

但是实测的时候，有一个非常严重的问题，就是输入gulp的时候，好像有时候可以有效，有时候没效果；（不知道是不是中间传输文件的时间原因；）

 

如果要传多个文件，可以直接多写一个*；

```javascript
var gulp = require('gulp');
var scp = require('gulp-scp2');
 
gulp.task('default', function () {
    gulp.src('js/**/*')
        .pipe(scp({
            host: '192.168.**.***',
            username: 'administrator',
            password: '*******',
            dest: '/d:/zhuanbang'
        }))
        .on('error', function(err) {
            console.log(err);
        });
});
```

js压缩，css压缩，并且实时压缩；附带scp功能；其中rsync没有调好；先不放进去；

```javascript
'use strict';
 
var gulp = require('gulp');
var scp = require('gulp-scp2');//上传到服务器，相当于全部文档全部复制
var rsync = require('gulp-rsync');//同步到服务器，相当于把修改后的文件同步到服务器；性能更好
var uglify = require('gulp-uglify');//获取 uglify 模块（用于压缩 JS）;
var minifyCSS = require('gulp-minify-css');//gulp-minify-css用于压缩CSS;
 
 
// 压缩 js 文件
// 在命令行使用 gulp js 启动此任务
gulp.task('js', function() {
    // 1. 找到文件
    gulp.src('src/js/*.js')
        // 2. 压缩文件
        .pipe(uglify())
        // 3. 另存压缩后的文件
        .pipe(gulp.dest('dist/js'))
});
 
// 压缩 css 文件
// 在命令行使用 gulp css 启动此任务
gulp.task('css', function () {
    // 1. 找到文件
    gulp.src('src/css/*.css')
        // 2. 压缩文件
        .pipe(minifyCSS())
        // 3. 另存为压缩文件
        .pipe(gulp.dest('dist/css'))
});
 
// 在命令行使用 gulp watch 启动此任务
gulp.task('watch', function () {
    // 监听文件修改，当文件被修改则执行 js，css 任务
    gulp.watch('src/js/*.js', ['js']);
    gulp.watch('src/css/*.css', ['css']);
});
 
 
// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 js,css 任务和 watch 任务
gulp.task('default', ['js','css','watch']);
 
 
//scp 直接复制到远程服务器
gulp.task('up', function () {
    gulp.src('../testPub/**/*')
        .pipe(scp({
            host: '192.168.**.***',
            username: 'administrator',
            password: '*******',
            dest: '/d:/zhuanbang'
        }))
        .on('error', function(err) {
            console.log(err);
        });
});
```