## 环境

```
npm install gulp@3.9.0 --save-dev
npm install gulp-babel@7 babel-core babel-preset-es2015 --save-dev
```

## 新建 `gulpfile.js` 文件
```
const gulp = require('gulp');
const uglify = require('gulp-uglify');
const babel = require('gulp-babel');


gulp.task('default',function(){
    /////找到文件
    gulp.src('./a.js')
    //////把ES6代码转成ES5代码
    .pipe(babel())
    /////压缩文件
    .pipe(uglify())
    /////另存压缩后文件
    .pipe(gulp.dest('dist/js'));
});
```

## 新建`.babelrc`文件

```
{
  "presets": ["es2015"]
}
```

## 运行

运行 `gulp` 
