---
title: webpack中多页面的配置  
date: 2018-08-25 12:28
categories: ['webpack']
type: ['webpack']
---
<br>
因为项目不是spa的单页面应用,只是基于`jquery`的项目,所以需要进行多页面的统一配置,
研究了一下其实大部分是`html-webpack-plugin`的配置,然后配合其他的使用
### 项目介绍
因为我们的项目是基于jquery的,其中部分是`iframe`的嵌套,为了浏览器的缓存问题,所以`jquery`和一些公共css的引用不能进行打包,
当然这种情况 `gulp`基本能满足我们的需求,但是为了学习一下`webpack`所以研究一下在webpack中如何进行同样的配置的;

### 原理
在`webpack`中的`plugin`中设置多个 `html-webpack-plugin`,每个实例都会生成新的页面通过设置依赖的`chunks`
最后依赖于不同包
比如我下面设置的
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin')

module.exports = {
    entry:{
        index:'./js/index.js',
        index1:'./js/index1.js',
    },
    output:{
        filename:'[name].bundle.js',
        path:path.resolve(__dirname,'dist')
    },
    plugins:[
        new CleanWebpackPlugin(['dist']),
        new HtmlWebpackPlugin({
            filename:'index.html',
            chunks:['index'],
            template:'./index.html'
        }),
        new HtmlWebpackPlugin({
            filename:'index1.html',
            chunks:['index1'],
            template:'./index.html'
        })
    ],
    module:{
        rules:[
            {
                test:/\.css$/,
                use:[
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
};
```
这样每次打包生成出来两个`hml`页面 其中的`template`部分为 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>this is test title</title>
    <script src="js/jquery-1.12.4.min.js"></script>
</head>
<body>
    <p>test webpack html-webpack-plugin template</p>
</body>
</html>
```
这是`htm-webpack-plugin`的模板
最后生成的类似于 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>this is test title</title>
    <script src="js/jquery-1.12.4.min.js"></script>
</head>
<body>
    <p>test webpack html-webpack-plugin template</p>
<script type="text/javascript" src="index.bundle.js"></script></body>
</html>
```
这样就实现了依赖于不同的包,但是引用的`jquery`有是同一个`jquery`,解决了缓存的问题,至于最后生成的js,我们需要设置一下生成的目录就行了.

### HtmlWebpackPlugin插件
有关配置项
```javascript
this.options = _.extend({
    template: path.join(__dirname, 'default_index.ejs'),
    filename: 'index.html',
    hash: false,
    inject: true,
    compile: true,
    favicon: false,
    minify: false,
    cache: true,
    showErrors: true,
    chunks: 'all',
    excludeChunks: [],
    title: 'Webpack App',
    xhtml: false
  }, options);
```
详细的信息 可以从 [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)上进行观看;

### 注意事项
如果使用`template`的时候,`option`中设置的一些东西可能无法生效,这时候就推荐使用模板引擎
比如 `ejs`,如下 是我们使用`ejs`渲染`option`中的`title`属性(注意:不同的模板引擎的语法不一样)
```ejs
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title><%= htmlWebpackPlugin.options.title %></title>
    <script src="js/jquery-1.12.4.min.js"></script>
</head>
<body>
    <p>test webpack html-webpack-plugin template</p>
</body>
</html>
```