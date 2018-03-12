---
title: gulp入门(一)
date: 2017-03-30 21.52
categories: gulp
tag: ['gulp']
---
# 安装gulp依赖
### 1首先应该全局安装gulp依赖
```javascript
npm install -g gulp
```
### 2.项目中生成package.json文件
```javascript
npm  init
```
`npm init` 是项目初始化时使用,会默认生成一个package文件
### 3.安装需要的包
常见的包的介绍<br>
`gulp`  gulp的包<br>
`gulp-uglify`  压缩js文件<br>
`gulp-minify-css`  压缩css文件<br>
`gulp-rename`  文件重命名<br>
`gulp-sourcemaps`  对压缩的文件生成sourcemaps文件,方便进行调试时对错误进行定位<br>
`gulp-sass`  把sass或者scss文件转成css<br>
`node-sass`  gulp-sass文件依赖,如果不安装的话有时候会报错`<br>

```javascript
npm install --save-dev gulp gulp-minify-css gulp-rename gulp-sourcemaps gulp-sass node-sass
```
有的时候如果一次性安装过多会报错,建议使用cnpm 或者一个一个的安装包<br>
有的node版本需要把`--save-dev`或者`--save`放在最后 即这样写

```javascript
npm install  gulp --save-dev
```
## 二gulp文件的写法
这个时候一般你的目录结构是这样的<br>
project<br>
--js<br>
--css<br>
--img<br>
--scss<br>
--node_modules<br>
--gulpfile.js<br>
--package.json<br>
--index.html

### 1.新建一个gulpfile.js文件
下面的内容是在gulpfile文件中写的<br>
引入之前的依赖模块
```javascript
var  gulp=require('gulp'),
     sass=require('gulp-sass'),
     minifycss=require('gulp-minify-css'),
     rename=require('gulp-rename'),
     uglify=require('gulp-uglify'),
     sourcemaps=require('gulp-sourcemaps');
```
### 2.建立一个默认的处理方法
在最新版中,你必须创建一个默认的处理,必须为`default`,否则会报错,无法执行
```javascript
gulp.task('default', function () {
  gulp.src('./scss/*.scss')  //需要处理的文件scss目录下所有的.scss文件
    .pipe(sass())              //使用sass模块进行处理
    .pipe(sourcemaps.write('./')) //生成sourcemaps文件
    .pipe(gulp.dest('./pai/css/'));//输出的目录
});
```

### 3.监听
新版本的监听与之前的监听不同,按照最新的写就行
```javascript
gulp.watch('./sass/*.scss',['default']);
//运行gulp的监听sass文件夹下的所有.scss文件,只要发生变化就执行'default'
```
### 4.css代码压缩
```javascript
gulp.task('default', function () {
  gulp.src('./css/*.css')  //输入目录
      .pipe(sourcemaps.init())  //生成sorrcemaps文件
      .pipe(minifycss())          //使用压缩css模块
      .pipe(rename({suffix:'.min'}))  //重命名
      .pipe(sourcemaps.write('./'))    //sourcemaps输出目录
      .pipe(gulp.dest('./css/'));  文件的输出目录
});
```
### 5.js代码压缩
```javascript
gulp.task('default', function () {
 gulp.src('./js/ctrl/*.js')
    .pipe(sourcemaps.init())
    .pipe(uglify())
    .pipe(rename({suffix:'.min'}))
    .pipe(gulp.dest('./minjs/'))
});
```