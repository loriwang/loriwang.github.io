---
title: angular自定义指令1
date: 2017-07-10 21:52
categories: angularJs
tag: ['angularJs']
---
## 背景 
新入职接手一个快上线的项目,最近一直在看项目的源代码,使用的是angular1的代码,其中自定义指令用的不能用很多来形容,所以感觉之前angular学的简直是个渣,所以开始更新一下angular中碰到的难点
## 自定义指令(`directive`)中的`scope`
### scoope的三种取值
#### 1.false()默认值,直接使用父`scope`,比较危险.
简单的来说就是指令内部没有一个新的`scope`,它和指令以外的代码共享同一个`scope`,即修改父`scope`会修改子`scope`,修改子`scope`会影响父`scooe`,
#### 2.true:继承父`scope`
即修改父类`scope`不会改变子`scope`的值,而修改子`scope`的值则会影响父`scope`的值.
一开始是绑定在父scope中，但当修改位于自定义指令中的输入框时，子scope就被创建并继承父scope了。之后，修改父scope并不能影响input的值，而修改子scope就可以改变input的值了
!['图片' '图片'](http://img.blog.csdn.net/20160815172838043)
#### 3.{};穿件一个新的'隔离'`scope`,但是仍然可以与`scope`通信;
隔离的`scope`,通常用来创建可服用的指令,也就是不用管父`scope`绑定的东西,然而虽然说是“隔离”，但通常我们还是需要让这个子scope跟父scope中的变量进行绑定。绑定的策略有3种：
- @：单向绑定，外部scope能够影响内部scope，但反过来不成立
- =：双向绑定，外部scope和内部scope的model能够相互改变
- &：把内部scope的函数的返回值和外部scope的任何属性绑定起来
##### 1.@单向绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>angular directive demo1</title>
    <script src="../js/angular.js"></script>
    <script>
        var app = angular.module('ngAppDemo',[]);
        app.directive('myDirective',function ($timeout) {
           return{
               restrict: 'E',
               scope: {
                   // @ 单向绑定 myText 是指在属性(my-text)里面绑定的东西
                   )
                   myText:'@'
               },
               replace: true,
            template:`<p>自定义指令scope:<input type="text" ng-model="myText"></p>`,
               controller:null
           }
        });
        app.controller('ngAppDemoController', function($scope) {
        })

    </script>
</head>
<body ng-app="ngAppDemo" ng-controller="ngAppDemoController">
<p>父scope:<input type="text" ng-model="input"></p>
<my-directive my-text="{{input}}"></my-directive>
</body>
</html>
```
!['图片' '图片'](http://img.blog.csdn.net/20160815171137022)
我们可以看到,在父元素修改的时候 可以改变子`scope`的值,子`scope`改变的时候父`scope`不会发生改变.
##### 2.=双向绑定
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>angular directive demo1</title>
    <script src="../js/angular.js"></script>
    <script>
        var app = angular.module('ngAppDemo',[]);
        app.directive('myDirective',function ($timeout) {
           return{
               restrict: 'E',
               scope: {
                   //@ 单向绑定
                   myText:'='
               },
               replace: true,
               template:`<p>自定义指令scope:<input type="text" ng-model="myText"></p>`,
               controller:null
           }
        });
        app.controller('ngAppDemoController', function($scope) {
        })

    </script>
</head>
<body ng-app="ngAppDemo" ng-controller="ngAppDemoController">
<p>父scope:<input type="text" ng-model="input"></p>
<my-directive my-text="input"></my-directive>
</body>
</html>
```
注意:<br>
在元素中使用属性，好比这样<div my-directive age="age"></div>,注意，数据的双向绑定要通过=前缀标识符实现，所以不可以使用`\{\{\}\}`。
!['图片' '图片'](http://img.blog.csdn.net/20160815171730353)
##### 3.&内部scope的函数返回值和外部scope绑定
这是一个绑定`函数方法`的前缀标识符<br>
使用方法：在元素中使用属性，好比这样`<div my-directive change-my-age="changeAge()"></div>`，注意，属性的名字要用-将多个个单词连接。
```javascript
scope: {
    changeAge: '&changeMyAge' 
}
```
注意:此处`&changeMyAge`后面的`changeMyAge`是在指令上的(`change-my-age`),然后前面的`changeAge`是我们命名的名字,这是当自定义指令上绑定多个指令的时候为了区分绑定的东西的不同