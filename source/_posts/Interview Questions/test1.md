---
title: 前端面试题(一)
date: 2017-04-18 17:42
categories: '面试题'
type: '面试题'
---
不定时更新,每次面试碰到的疑难的问题都会记录下来.一是给大家分享分享,二也是给自己提个醒,有时候面试题没你想的那么简单
# 一.输出foo的值,或语句中得到的Boolean值,语句的值
```javascript
(window.foo||(window.foo='bar'))
```
简单来看一下这个题,当`windw.foo`存在的时候不变,否则给`window.foo`赋值`bar`<br>
那么这个语句的`boolean`值为true;<br>
最后的问题来了,输出这个语句 最后的结果是什么呢??<br>
答案是`bar`<br>
为什么跟我们一开始想象的不是太一样呢?不应该是输出`boolean`值`true`么?<br>
采取倒推的思想,我觉得可能是 最后输出的是`window.foo`的值,而不是或语句判断的值

# 二.立即执行函数的问题(闭包) 最后的输出结果
```javascript
var foo='hello';
(function() {
  var bar='world';
  console.log(foo+bar)
})()
  console.log(foo+bar)
```
这个问题稍微对这个立即执行函数了解的都知道,会输出两次,我原来想的是  第二次输出应该是`hello undefined`,回来之后敲了一下代码发现是报错`Uncaught ReferenceError: bar is not defined`,
就是`bar`没有定义;<br>
这个问题自己还要注意一下,`undefined`与`未定义`是两个概念,`未定义`会直接报错的

# 循环问题
```javascript
            for(var i=0;i<10;i++){
                setTimeout(function () {
                    console.log(i)
                },0)
            }
```
最后的答案也不用多说了 最后输出10次`10`;原因就是 虽然延时器为0;但是还是没有for循环快(注:自己理解的);

# 输出index
```html
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
```
第一种方法
```javascript
            var oUl=document.getElementsByTagName('ul')[0];
            var oLis=oUl.getElementsByTagName('li')
            for(var i=0;i<oLis.length;i++){
                (function (i) {
                    oLis[i].onclick=function () {
                        alert(i)
                    }
                })(i)
            }

```
第二种方法
```javascript
            for(var i=0;i<oLis.length;i++){
                    oLis[i].onclick=(function (i) {
                       return function () {
                           alert(i)
                       }
                    })(i)

            }
```
第二种方法面试的时候没写出来.比较遗憾,面试官说有更好的解决方法,事件委托,这里我也查了一下<br>
```javascript
            var oUl=document.getElementsByTagName('ul')[0];
            var oLis=oUl.getElementsByTagName('li');
            var liNodes=Array.prototype.slice.call(oLis);
            console.log(liNodes)
            console.log(oLis)
            for(var i=0;i<oLis.length;i++){
                oLis[i].addEventListener('click',function () {
                    var event = event || window.event;
                    var target = event.target || event.srcElement;
                    console.log(liNodes.indexOf(target))
                },false)
            }
```
当初还以为在想 事件委托没办法的到呢  因为没有属性,现在想想 还是too young too simple,知识点理解的太浅了
# 原型问题[1,2,3,4,5].double()  输出[1,2,3,4,5,1,2,3,4,5];
对原型的添加方法不是太了解,没想出来,回来后查了查直接贴上来了
```javascript
            var  arr=[1,2,3,4,5];
            Array.prototype.double=function () {
                return this.concat(this)
            }
            console.log(arr.double())
```
# 三.死循环问题
```javascript
var t = true;
window.setTimeout(function () {
    t = false;
},10000);
alert(t);
while (t) {

};
console.log(t);
//  两次输出的结果,并详细解释
//  原本的问题是没有第一个alert的 不过为了大家了解,我加了这一条
```
这个问题考验的问题,一个是js的执行顺序,一个是延迟函数`setTimeout`,还有一个是`while`死循环的问题,这个问题的结果是只有第一次弹出一个`true`,第二次浏览器直接卡死,不会输出结果,原因就是在`setTimeout`的时候不会影响下一步的执行,卡死的原因则是`while`进入了死循环<br>
如果把延迟的`10000`秒换成`1`会不会跳出死循环,答案是不会,
其他的特别的问题就没了



<p align=right>---by  lori.Wang</p>
