---
title: Object.create(null) 和 {} 的区别 
date: 2018-12-09 12:52
categories: 'javascript'
type: 'javascript'
---

我们在看一些源码的时候经常发现库的作者会经常使用 `Object.create(null)`而不是`{}`字面量进行定义,两者有什么区别呢?
```javascript
  let m = Object.create(null);
   let n = {};

   console.log(m);
   console.log(n);
```
!['图片','图片'](https://user-gold-cdn.xitu.io/2018/12/7/16787f5aa3199c77?imageView2/0/w/1280/h/960/format/webp/ignore-error/1) 

我们看到`Object.create(null)`生成的就是一个空对象,不存在对象所有的方法,不指向任何原型<br>
而使用字面量进行定义呢
!['图片','图片'](https://user-gold-cdn.xitu.io/2018/12/7/16787f516f31bdef?imageView2/0/w/1280/h/960/format/webp/ignore-error/1) 
我们发现它集成了对象原型的空对象,

```
Object.create(proto, [propertiesObject]) 方法

proto 新建对象的原型对象
propertiesObject 可选，添加到新建对象的属性（不是原型链的属性），默认可枚举，参数对应Object.defineProperties的第二个参数
```

所以我们可以理解为 `{}`相当于`Object.create(Object.ptototype)`