---
title: angular国际化
date: 2017-07-10 21:52
categories: angularJs
tag: ['angularJs']
---
## 什么是angular-translate
其实就是把网站做成国际化,多语言切换
## 一.安装 angular-translate
我采用的是`npm`安装方式<br>
```
npm install --save angular-translate
```
## 二.引入 
正规的引入即可
```html
  <script src="js/angular.js"></script>
  <script src="js/angular-translate.js"></script>
```
## 三.注入
```javascript
    var app = angular.module('ngAppDemo', ['pascalprecht.translate'])
```
## 四.配置
我们在`app.config`中进行多语言的配置
```javascript
    app.config(['$translateProvider', function($translateProvider) {
        $translateProvider.translations('en',{
          "searchText": 'place insaert searchText',
          "languageSwitch":'switch language'
        })
        $translateProvider.translations('cn',{
          "searchText": '请输入搜索内容',
          "languageSwitch": "切换语言"
        })
        $translateProvider.preferredLanguage('en')
    }])
```
上述代码中,`$translateProvider.preferredLanguage('en')`是初始语言的选择<br>
`$translateProvider.translations`这个是语言的配置,我们可以看到,我们在这里配置了`en`和`cn`两个<br>
其中的`searchText`和`languageSwitch`是我们配置的语言的`key`,那在我们需要进行国际化的地方 我们直接放入对应的`key`即可

## 五.使用
首先我们定义一个`controller`
```javascript
    app.controller('ngAppDemoController', function($translate,$scope) {
      var vm = this;
    vm.switchLanguage = function(){
          $translate.use('cn')
      };
    });
```
我们再配置一下`html`中的内容
```javascript
<body ng-app="ngAppDemo" ng-controller="ngAppDemoController as vm">
  <div >
    <label><input type="text" ng-model="vm.keyWord" placeholder="{{'searchText' | translate}}"><button ng-click="vm.keyWord = ''">删除</button></label> {{vm.keyWord}}
  </div>
  <button ng-click='vm.switchLanguage()'>{{'languageSwitch' | translate}}</button><br/>
  {{'searchText' | translate}}<br />
</body>
```
首先说明一下在`html`中的使用,我们通过类似过滤器(filter)的使用方式来使用,只是里面的内容需要你填入的是需要国际化的`key`比如:<br>
我们的`button`按钮,我们写入的是内容是`\{\{'languageSwitch' | translate\}\}`,注意我们写入的`languageSwitch`是带有引号的,说明这是一个`key`而不是字符串,后面跟的`translate`是用来把对应的key翻译成对应的语言的.<br>
而我给button绑定个事件,是用来切换语言通过` $translate.use\(\)`方法.来切换语言
## 六.全部代码
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>demo</title>
  <script src="js/angular.js"></script>
  <script src="js/angular-translate.js"></script>
  <script>
    var app = angular.module('ngAppDemo', ['pascalprecht.translate'])

    app.config(['$translateProvider', function($translateProvider) {
        $translateProvider.translations('en',{
          "searchText": 'place insaert searchText',
          "languageSwitch":'switch language'
        })
        $translateProvider.translations('cn',{
          "searchText": '请输入搜索内容',
          "languageSwitch": "切换语言"
        })
        $translateProvider.preferredLanguage('en')
    }])
    app.controller('ngAppDemoController', function($translate,$scope) {
      var vm = this;
      vm.text = 'vm text'
    vm.switchLanguage = function(){
          $translate.use('cn')
      };
    });
  </script>
</head>

<body ng-app="ngAppDemo" ng-controller="ngAppDemoController as vm">
  <div >
    <label><input type="text" ng-model="vm.keyWord" placeholder="{{'searchText' | translate}}"><button ng-click="vm.keyWord = ''">删除</button></label> {{vm.keyWord}}
  </div>
  <button ng-click='vm.switchLanguage()'>{{'languageSwitch' | translate}}</button><br/>
  {{'searchText' | translate}}<br />
</body>

</html>
```
## 六.开发中的使用
在实际的开发中,我们不可能把大把的语言写入到`config`中,我们是写入到`json`文件中,不同的语言对应不同的文件,一般是放到同级目录下的'i18n`文件夹下比如我们公司的目录配置
```
i18n
---locale-en_US.json
---locale-zh_cn.json
```
我们可以引入另外一个东西`angular-translate-loader-static-files`
## 七.angular-translate-loader-static-files
这个是用来读取静态资源的,也就是读取我们的语言包<br>
### 1.注入
因为这个暴露出来的东西跟`angular-translate`一样,所以注入的时候只注入`angular-translate`的就行
即
```javascript
var app = angular.module('ngAppDemo', ['pascalprecht.translate'])
```
### 2.使用
```javascript
    app.config(['$translateProvider', function($translateProvider) {
        // $translateProvider.translations('en',{
        //   "searchText": 'place insaert searchText',
        //   "languageSwitch":'switch language'
        // })
        // $translateProvider.translations('cn',{
        //   "searchText": '请输入搜索内容',
        //   "languageSwitch": "切换语言"
        // })
        $translateProvider.useStaticFilesLoader({
              files:[{
                prefix:'./i18n/locale-',
                suffix:'.JSON'
              }]
        })
        $translateProvider.preferredLanguage('en_US')
    }])
```
我们注释掉之前东西,现在语言的读取是从`./i18n/`文件夹下读取的,同事会自动添加上文件的名字,如我们在设置首选语言的时候我们只写入了`en_US`那么在读取文件的时候,会自动拼接成文件的名字即`./i18n/locale-en_US.json`文件.
同样在切换输入法的时候 也只需要写入后面的名字即可
```javascript
    vm.switchLanguage = function(){
          $translate.use('zh_CN')
      };
```
### 3.注意
在未读取到对应的文件或`key`值的时候,会自动填入`key`即 如果找不到`searchText`那么会直接显示`searchText`.
## 八.$translate的使用
有的时候我们在js中也要用到提示,比如弹出框,我们需要根据用户选择语言的不同去提示不同的语言,这里我们就要用到`$translate`了,使用方法是.
```javascript

```
## 七.translate过滤器的使用
使用中有三种使用方法
### 1.页面中直接使用
```html
{{'searchText' | translate}}
```
### 2.js中使用过滤器
```javascript
       let TranslateKey = ["pullKey_01","pullKey_02","pullKey_03"];
        TranslateText = $filter('translate')(TranslateKey);
```
### 3.js中第二种使用方式
```javascript
var  $translate = $filter('translate')
instructionsPullToRefresh: $translate('pullKey_01')
```
### 八.使用中的注意点
当我们传进去一个数组的时候,返回的是一个对象<br>
如
```javascript
       let TranslateKey = ["pullKey_01","pullKey_02","pullKey_03"];
        TranslateText = $filter('translate')(TranslateKey);
        //
```

