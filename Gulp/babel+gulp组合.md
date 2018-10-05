安装
```
npm install gulp gulp-babel –save-dev
```
创建gulp的配置文件 gulpfile.js
```
touch gulpfile.js
```
配置文件如下
```javascript
var gulp=require('gulp');
var babel=require('gulp-babel');
    
gulp.task("babel",function () {
    return gulp.src('src/*.js')
        .pipe(babel())
        .pipe(gulp.dest('build'))
});
    
gulp.task("default",["babel"]);
```
在 package 里配置命令
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "./node_modules/.bin/babel src -wd build",
    "dev": "./node_modules/.bin/gulp"
},
```
上面新增了dev这个命令；
```
npm run dev
```
此时也可以达到编译效果；（类似babel的）