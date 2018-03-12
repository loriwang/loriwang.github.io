---
title: gulp中postCss使用
date: 2017-04-25 08:25
categories: 'gulp'
type: 'gulp'
---
最近在学习vue,发现vue-cli中有使用postCss,所以正好在gulp中也使用一下postCss<br>
## 准备工作
* node.js
* npm
* gulp
## 一.安装
```
npm install --save-dev gulp-postcss
```
## 二.引用
在gulpfile.js中引入
```javascript
var gulp=require('gulp');
var postcss=require('gulp-postcss')
```
## 三.使用
```javascript
    var processors = [];
    return gulp.src('./postcss/*.css')
        .pipe(postcss(processors))
        .pipe(gulp.dest('./css'))
```
其中`processors`是我们要使用的插件<br>
现在我们运行一下
```
gulp default
```
现在我们看到 在css目录下生成了一个css文件,但是并没有发生任何改变,这是因为我们还没有使用`postcss`的插件<br>
现在我们来使用`postcss`的插件
## 四.postcss插件
现在我们添加需要的PostCSS插件：Autoprefixer(处理浏览器私有前缀)，cssnext(使用CSS未来的语法),precss(像Sass的函数)。
```
npm install autoprefixer --save-dev 
npm install cssnext --save-dev 
npm install precss --save-dev
```
其实也可以在命令中执行下面的代码，也可以达到相同的效果：
```
npm install autoprefixer cssnext precss --save-dev
```
但是这种使用方法有的时候会很慢,建议心急的用户使用第一种
## 五.postcss插件的使用
引入<br>
```javascript
var autoprefixer=require('autoprefixer')
var cssnext=require('cssnext');
var precss=require('precss')
```
使用:<br>
```javascript
gulp.task('postcss',function () {
    var processors = [
        autoprefixer,
        cssnext,
        precss
    ];
    return gulp.src('./postcss/*.css')
        .pipe(postcss(processors))
        .pipe(gulp.dest('./css'))
})
```
我们测试一下
```css
/* Testing autoprefixer */
 .autoprefixer { 
 display: flex; 
 } /* Testing cssnext */ 
 .cssnext {
  background: color(red alpha(-10%)); 
  } /* Testing precss */ 
  .precss {
   @if 3 < 5 { 
   background: green;
    } 
    @else { 
    background: blue; 
    }
     }
```
输出结果:
```css
/* Testing autoprefixer */ 
.autoprefixer { 
display: -webkit-box; 
display: -webkit-flex;
 display: -ms-flexbox; 
 display: flex; 
 } /* Testing cssnext */
  .cssnext { 
    background: rgba(255, 0, 0, 0.9);
   } /* Testing precss */ 
   .precss { 
    background: green
    }
```
如上面编译出来的代码你应该看到了`Autoprefixer`给需要的属性添加了浏览器的私有前缀，第二个任务`cssnext`将颜色转换成`rgba()`，第三部分PreCSS检查了`@if @else`，编译符合需求的代码。
## 六.更多插件
可以去github上去查看
[postcss](https://github.com/postcss/postcss)

<p align=right>---by  lori.Wang</p>