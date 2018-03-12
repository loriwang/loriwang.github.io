---
title: AMD的requireJs(一)
date: 2017-05-05 15:28
categories: ['javascript','AMD/CMD']
type: ['javascript','AMD/CMD']
---
## 一.为什么要用requireJs
最早的时候，所有Javascript代码都写在一个文件里面，只要加载这一个文件就够了。后来，代码越来越多，一个文件不够了，必须分成多个文件，依次加载。下面的网页代码，相信很多人都见过。
```html
    <script src="1.js"></script>
　　<script src="2.js"></script>
　　<script src="3.js"></script>
　　<script src="4.js"></script>
　　<script src="5.js"></script>
　　<script src="6.js"></script>
```
不只是以前,现在很多项目我们都还是用这种写法来引入js和css的<br>
这段代码依次加载多个js文件。<br>
这样的写法有很大的缺点。首先，加载的时候，浏览器会停止网页渲染，加载文件越多，网页失去响应的时间就会越长；其次，由于js文件之间存在依赖关系，因此必须严格保证加载顺序（比如上例的1.js要在2.js的前面），依赖性最大的模块一定要放到最后加载，当依赖关系很复杂的时候，代码的编写和维护都会变得困难。
<br>
使用RequireJs就是为了解决这种问题的
## 二.require的加载
[下载地址](http://requirejs.org/docs/download.html#requirejs)<br>
像普通的js一样进行加载,如下:`require.js`文件放在js目录下的
```html
<script src="js/require.js"></script> 
```
加载这个文件，也可能造成网页失去响应。解决办法有两个，一个是把它放在网页底部加载，另一个是写成下面这样：
````html
<script src="js/require.js" defer async="true" ></script>
````
async属性表明这个文件需要异步加载，避免网页失去响应。IE不支持这个属性，只支持defer，所以把defer也写上。<br>
加载`require.js`以后，下一步就要加载我们自己的代码了。假定我们自己的代码文件是`main.js`(传说中的入口文件)，也放在js目录下面。那么，只需要写成下面这样就行了<br>
```html
<script src="js/require.js" data-main="js/main"></script>
```
`data-main`属性的作用是，指定网页程序的主模块。在上例中，就是js目录下面的`main.js`，这个文件会第一个被`require.js`加载。由于`require.js`默认的文件后缀名是js，所以可以把`main.js`简写成`main`。<br>
## 三.主模块的写法
如果我们的代码不依赖任何其他模块，那么可以直接写入javascript代码。
```javascript
//main.js
alert('加载完成')
```
当然也可以进行dom操作
```javascript
let btn=document.getElementById('btn');
btn.onclick = function () {
    alert(1)
}
```
但是如果知识这样的话我们还没必要用`RequireJs`了;<br.
。真正常见的情况是，主模块依赖于其他模块，这时就要使用AMD规范定义的的require()函数。<br>
```javascript
　　// main.js
　　require(['moduleA', 'moduleB', 'moduleC'], function (moduleA, moduleB, moduleC){
　　　　// some code here
　　});
```
require()函数接受两个参数。第一个参数是一个数组，
表示所依赖的模块，上例就是`['moduleA', 'moduleB', 'moduleC']`，
即主模块依赖这三个模块；第二个参数是一个回调函数，
当前面指定的模块都加载成功后，它将被调用。加载的模块会以参数形式传入该函数，
从而在回调函数内部就可以使用这些模块。<br><br>
require()异步加载moduleA，moduleB和moduleC，浏览器不会失去响应；它指定的回调函数，只有前面的模块都加载成功后，才会运行，解决了依赖性的问题。<br>
<br><br>
假定主模块依赖jquery、underscore和backbone这三个模块，main.js就可以这样写：
```javascript
require(['jquery', 'underscore', 'backbone'], function ($, _, Backbone){
　　　　// some code here
　　});
```
require.js会先加载jQuery、underscore和backbone，然后再运行回调函数。主模块的代码就写在回调函数中。
## 四.模块的加载
上一节最后的示例中，主模块的依赖模块是`['jquery', 'underscore', 'backbone']`。默认情况下，`require.js`假定这三个模块与`main.js`在同一个目录，文件名分别为`jquery.js`，`underscore.js`和`backbone.js`，然后自动加载。<br>
但是在我们使用的时候后都会使用压缩后的代码文件比如`jquery.min.js`这个时候我们就要配置模块的加载路径了<br>
我们使用`require.config()`进行配置,`require.config()`放在入口文件的头部,即我们先配置再使用
```javascript
require.config({
    path:{
        　"jquery": "jquery.min",
        　"underscore": "underscore.min",
        　"backbone": "backbone.min"
    }
});
require(['jquery', 'underscore', 'backbone'], function ($, _, Backbone){
　　　　// some code here
　　});
```
当然文件前面也可以带上文件的所在目录
```javascript
　require.config({
　　　　paths: {
　　　　　　"jquery": "lib/jquery.min",
　　　　　　"underscore": "lib/underscore.min",
　　　　　　"backbone": "lib/backbone.min"
　　　　}
　　});
```
这样的文件也是在`js`目录下面的,即`jquery.min.js`这个文件是在与`main.js`文件同级的子目录`lib`下面的<br>
我们也可以改变基准目录
```javascript
require.config({
　　　　baseUrl: "js/lib",
　　　　paths: {
　　　　　　"jquery": "jquery.min",
　　　　　　"underscore": "underscore.min",
　　　　　　"backbone": "backbone.min"
　　　　}
　　});
```
也可以指定网址
```javascript
　require.config({
　　　　paths: {
　　　　　　"jquery": "https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min"
　　　　}
　　});
```
## 五.AMD模块的写法
前面我们讲的是如何使用模块,那么我们如何写模块呢?`require.js`加载的模块是采用AMD规范.所以我们的模块也必须按照AMD的规定来写<br>
具体来说,就是模块必须采用特定的`define()`函数来定义,如果不依赖于其他模块,那么可以直接定义在`define()`函数之中.<br>
假如有个`math.js`文件,它定义了一个math模块,那么就应该
```javascript
//math.js
define(function() {
  var add=function(x,y) {
    return x + y;
  };
  return {
      add:add
  }
})
```
加载方法如下:
```javascript
require(['math'],function(math) {
  alert(math.add(1,1));
})
```
如果这个模块还依赖其他模块，那么define()函数的第一个参数，必须是一个数组，指明该模块的依赖性。<br>
```javascript
　define(['myLib'], function(myLib){
　　　　function foo(){
　　　　　　myLib.doSomething();
　　　　}
　　　　return {
　　　　　　foo : foo
　　　　};
　　});
```
当require()函数加载上面这个模块的时候，就会先加载myLib.js文件
## 六、加载非规范的模块
理论上，require.js加载的模块，必须是按照AMD规范、用`define()`函数定义的模块。但是实际上，虽然已经有一部分流行的函数库（比如jQuery）符合AMD规范，更多的库并不符合。那么，`require.js`如何加载非规范的模块呢?<br>
我们需要在`require()`加载之前,要先用`require.config()`定义他们的一些特征<br>
举例来说，underscore和backbone这两个库，都没有采用AMD规范编写。如果要加载它们的话，必须先定义它们的特征。
```javascript
require.config({
    shim: {
        'underscore': {
            exports: '_'
        },
        'backbone': {
            deps:['underscore','jquery'],
            exports:'Backbone'
        }
    }
})
```
require.config()接受一个配置对象，这个对象除了有前面说过的paths属性之外，还有一个shim属性，专门用来配置不兼容的模块。<br>
具体来说，每个模块要定义（1）`exports`值（输出的变量名），表明这个模块外部调用时的名称；<br.
（2）`deps`数组，表明该模块的依赖性。<br>
比如，jQuery的插件可以这样定义：
```javascript
　　shim: {
　　　　'jquery.scroll': {
　　　　　　deps: ['jquery'],
　　　　　　exports: 'jQuery.fn.scroll'
　　　　}
　　}
```
## 七.require.js插件
domready插件，可以让回调函数在页面DOM结构加载完成后再运行。
```javascript
　　require(['domready!'], function (doc){
　　　　// called once the DOM is ready
　　});
```




<p align=right>---by  lori.Wang</p>