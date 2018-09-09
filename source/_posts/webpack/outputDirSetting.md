---
title: webpack中输出目录的设置
date: 2018-09-08 22:28
categories: ['webpack']
type: ['webpack']
---
<br><br>

我们项目中最后的打包可能要使不同的文件输出到不同的目录下,比如`js`文件统一到`dis/js`文件夹下,`ccs`到`dist/css`下等等,
所以我们看一下如何配置才能使文件输出到不同的目录下 
<br/>

基础配置
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin')

module.exports = {
        entry:{
            index:'./js/index.js',
        },
            output:{
                filename:'[name].bundle.js',
                path:path.resolve(__dirname,'dist')
            },
        plugins:[
            new CleanWebpackPlugin(['dist']),
            ],
        module:{
        
        }
            
}

```
## 1.配置js
```javascript
    output:{
        filename:'js/[name].bundle.js',
        path:path.resolve(__dirname,'dist')
    }
```

## 2.配置html
```javascript
        new HtmlWebpackPlugin({
            filename:'../index.html',
            title:'this is  index',
            chunks:['index'],
            template:'index.ejs'
        })
```
因为我们打包的目录为`dist`,如果是`./index.html`,则最后输出的目录为`dist/index.html`,如果是`../index.html`,则输出的与`dist`文件夹同级,
所以这里如果不是指定目录的话为`index.html`,或者`index.html`,

## 3.配置图片输出目录
```javascript
{
    test:/ \.(jpg|png|gif|jpeg)$/,
    use:[{
        loader:'url-loader',
        options:{
            limit:10000,
            name:'img/[name]-[hash:6].[ext]'
        }
    }]
}
```
其中[name]是图片的名字，[ext]是之前的后缀名。

## 4.配置字体输出目录
```javascript
{
    test:/\.(ttf|eot|woff|woff2|svg)/,
    use:[{
        loader:'file-loader',
        options:{
            limit:10000,
            name:'fonts/[name]-[hash:6].[ext]'
        }
    }]
}
```

## 5.配置css输出目录
由于css在webpack中是以内联样式存在的,所以如果我们需要吧css单独输出的话,需要增加额外的`plugin` `extract-text-webpack-plugin`,
当然在`webpack4.x`版本中官方是这么说的<br>

>：警告：由于webpack v4 `extract-text-webpack-plugin`不应该用于css。请改用`mini-css-extract-plugin`。

所以看到这个提示 你懂得!!
下面我们先看 `extract-text-webpack-plugin` 在 `webpack4.x`以下的

````javascript
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
      plugins: [
        new ExtractTextPlugin("css/styles.css"),
        // 这里的css/ 就是输出在 css目录下
      ]
}
````

在 `webpack4.x`版本中

```javascript
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
        plugins:[
        new MiniCssExtractPlugin({
            // Options similar to the same options in webpackOptions.output
            // both options are optional
            filename: "css/[name].css",
            chunkFilename: "css/[id].css"
        }),
        ],
            module:{
                 rules:[
                     {
                         test:/\.css$/,
                         use:[
                             {
                               loader:MiniCssExtractPlugin.loader
                             },
                             'css-loader'
                         ]
                     }
                 ],
             }
}

```