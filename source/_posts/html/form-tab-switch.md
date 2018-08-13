---
title: 表单中tab按钮不选中某些元素
date: 2017-07-11 18:34
categories: ['html']
type: ['html']
---
# 表单中tab切换
我们在使用表单的时候最常见的是这样的结构
!['图片' '图片'](http://oncoxe8eo.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170711183655.png)
那么当用户进行填写登录的表单的时候在pc端会经常使用tab来进行输入框的切换.<br>
当用户填写完密码的时候,摁下`tab`则会吧焦点移动到`忘记密码`那里,这个时候我们要怎么解决这个问题呢.<br>
在html中 有个属性叫做`tabindex`,是专门用来设定tab切换的顺序的,当所有的input标签中都没有这个属性的时候,它会按照页面上的顺序来进行切换
## tabindex的值
tabindex的值有三种情况
-    正值,范围为(1-32767)之间,按照从小到大顺序切换
-    0 ,最后执行即顺序为(1 2 3 4 32767 0)
-   -1,不会被tab获取焦点
下面我们来举个栗子
```html
<a href="http://webaim.org/" tabindex="1">tabindex1</a>
<form action="submit.htm" method="post">
<input type="submit" id="submitform1" tabindex="-1" value="tabindex-1">
<input type="submit" id="submitform0" tabindex="0" value="tabindex0">

<label for="name">Name</label>
<input type="text" id="name" tabindex="2">
<input type="submit" id="submitform" tabindex="3" value="tabindex3">
</form>
```
这样的摁`tab`按键 获取焦点的顺序为 1  2 3  0 ,`tabindex=-1`的不会被`tab`获取焦点<br>
所以我们可以按照我们的需求来决定用户tab切换的顺序
<p align=right>---by  lori.Wang</p>