---
title: webpack中通用模块的配置
date: 2018-09-09 01:27
categories: ['webpack']
type: ['webpack']
---
<br>
我们在写js的时候,可能有些通用的模块需要单独拎出来,这个时候我们如果不进行通用模块的配置的话,可能就会出现重复打包的事情,增大文件的体积,
因为自己目前使用的`webpack4.x`的版本,所以目前暂只说明4.x版本的,之后如果用到再会说 4.x以下版本的

## 配置 
主要是在 `optimization`中进行配置,目前的配置为
```javascript
module.exports = {
        entry:{
            index:'./js/index.js',
            index1:'./js/index1.js',
        },
        output:{
            filename:'js/[name].bundle.js',
            path:path.resolve(__dirname,'dist'),
            // 公共包会打成 chunk 所以这里是设置公共包的名称的
            chunkFilename: "js/[name].chunk.js"
        },
        // webpack4.x中使用 optimization 进行配置
        optimization:{
            // 设置公共包的部分  splitChunks
            splitChunks:{
                // 设置缓存的chunks
                cacheGroups:{
                    // 打包公共代码
                    common: {
                        name: "common-[id]",
                        chunks: "all",
                        minSize: 1,
                        // private 是针对于第三方的库 (例如lodash) 通过设置priority设置来让其先被打包提取,最后再提取剩余代码
                        priority: 0
                    },
                    // 打包node_modules 中的文件  或者 重复出现的代码
                    vendor: {
                        name: "vendor-[id]",
                        test: /[\\/]node_modules[\\/]/,
                        chunks: "all",
                        priority: 10
                    }
                }
            }
        },
        plugins:[
                new CleanWebpackPlugin(['dist']),
                new MiniCssExtractPlugin({
                    filename: "css/[name].css",
                    // 公共的css 也会打成chunk包  所以这里也要设置 
                    chunkFilename: "css/[name].css"
                }),
                 new HtmlWebpackPlugin({
                     filename:'index.html',
                     title:'this is  index',
                     // 这里代表改页面需要引入的js文件  包括 index 与common
                     chunks:['index','common'],
                     template:'index.ejs'
                 }),
                 new HtmlWebpackPlugin({
                     filename:'index1.html',
                     title:'this is  index1',
                     chunks:['index1','common'],
                     template:'index.ejs'
                 })
                ]
}
```
## splitChunks中配置项
- automaticNameDelimiter `string`
webpack将使用块的名称和名称生成名称（例如vendors~main.js）。此选项允许您指定用于生成的名称的分隔符。

- chunks `function` `string`
这表示将选择哪些块进行优化。如果提供一个字符串，可能的值是all，async和initial。

- maxAsyncRequests `number`
按需加载时的最大并行请求数。
- maxInitialRequests `number`
入口点处的最大并行请求数。
- minChunks `number`
分割前必须共享模块的最小块数。
- minSize `number`
要生成的块的最小大小（以字节为单位）。
- maxSize `number`
maxSizeoptions用于HTTP / 2和长期缓存。它增加了请求数以获得更好的缓存。它还可用于减小文件大小以加快重建速度。
- name  `boolean:true` `function` `string`
拆分块的名称。提供true将基于块和缓存组密钥自动生成名称。提供字符串或函数将允许您使用自定义名称。如果名称与入口点名称匹配，则将删除入口点。


### cacheGroups中的配置项
缓存组可以继承和/或覆盖任何选项splitChunks.*; 但是test，priority并且reuseExistingChunk只能在高速缓存组级别配置。要禁用任何默认缓存组，请将其设置为false。
- priority `number`
缓存组的优先级
模块可以属于多个缓存组。优化将更喜欢具有更高的缓存组priority。默认组的优先级为负，以允许自定义组采用更高的优先级（默认值适用于自定义组）。
- reuseExistingChunk  `boolean`
如果当前块包含已从主块拆分的模块，则将重用它而不是生成新的块。这可能会影响块的结果文件名。
- test `function` `RegExp` `string`
控制此缓存组选择的模块。省略则选择所有模块。

还有其他的可以观看webpack的官网[webpack-splitChunksPlugin](https://webpack.js.org/plugins/split-chunks-plugin/#src/components/Sidebar/Sidebar.jsx)