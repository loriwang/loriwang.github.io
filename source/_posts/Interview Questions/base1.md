---
title: 前端面试题基础(一)
date: 2017-05-02 17:31
categories: ['面试题']
type: ['面试题']
---
## 1.块级元素,行内元素,空元素
##### 常见块级元素
`div`,`h1`-`h6`,`p`,`form`,`ul`,`ol`,`li`,`table`,`caption`,`thead`,`tbody`,`tfoot`,`tr`,`td`,`dl`,`dt`,hr`
##### 常见行内元素(内联元素)
`a`,`b`,`i`,`span`,`label`,`em`,`select`,`input`,`img`,`textarea`,`strong`
##### 常见空元素
`link`,`br`,`meta`
## 2.常见的浏览器内核及兼容写法
 + 1.ie浏览器<br>
 使用的是`trident`内核
 + 2.火狐浏览器<br>
 使用的是`Gecko`内核,兼容写法`-moz-`
 + 3.谷歌和safari浏览器<br>
 使用的是`webkit`内核,兼容写法`-webkit-`
 + 4.欧朋浏览器
 使用的是`presto`内核,兼容写法`-o-`
## 3.ie浏览器的hack
```css
    .bb{
       background-color:#f1ee18;/*所有识别*/
      .background-color:#00deff\9; /*IE6、7、8识别*/
      +background-color:#a200ff;/*IE6、7识别*/
      _background-color:#1e0bd1;/*IE6识别*/ 
      } 
```
## 4.怎么理解mvc与mvvm

 #### mvc
 MVC (Model-View-Controller)<br>
  + 视图（View）：用户界面。
  + 控制器（Controller）：业务逻辑
  + 模型（Model）：数据保存<br>
  - view 传送指令到 Controller
  - Controller 完成业务逻辑后，要求 Model 改变状态
  - Model 将新的数据发送到 View，用户得到反馈<br>
  
 MVC是将Model, View和Controller分离，让彼此的职责(responsibility)能够明确的分开<br>
 这样不论是改M, V还是C，都可以确保另外两层可不用做任何修改，同时这样的分层也可以加强程式的可测试性(testability)<br>
 View和Model基本上是相关的，但它们并不会有直接的相依关系，而是由Controller去决定Model产生的资料，然后丢给View去做呈现<br>
 Controller是Model和View之间的协调者(coordinator)，View和Model不能直接沟通，以确保责任的分离。而Controller可以只是一个系结Model和View的小类别，也可以是大到包含Workflow, Enterprise Services或是做为外部系统的Proxy Services等的逻辑系统，MVC各元件是可以分离的组件，也可以是分离的系统(当然要设计一些机制在相互沟通)
 #### mvvm
 MVVM (Model-View-ViewModel)<br>
MVVM的架构一样是M, V分离，但中间是以VM (ViewModel)来串接<br>
这个ViewModel比较像是View的一个代理程式，它负责直接对Model做沟通，而View可以透过一些机制(ex: Events, Two-way Databindings, ...)来和ViewModel沟通以取得资料或将资料抛给Model做存取等工作，ViewModel也可以作为和外部系统的代理程式<br>
不过它和MVC不同的地方，就是ViewModel和View的黏合度比较高，因为View必须要透过ViewModel才可以取得Model,而ViewModel又必须要处理来自View的通知讯息，所以虽然职责一样分明，但是却不像MVC那样可以扩展到整个系统元件都能用。<br>
## 5.两种事件问题
直接在元素上写事件如
```html
<button onclick='tan()'><button>
```
和在js中获得元素再绑定事件有什么区别
答:<br>
在元素上写事件是HTML事件处理程序<br>
另外的是DOM事件处理程序

<p align=right>---by  lori.Wang</p>