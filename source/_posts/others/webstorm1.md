---
title: webstrom 热加载问题
date: 2017-4-24-24
categories: 'webstrom'
type: 'webstorm'
---
昨天把webstorm更新到了2017.1版本,更新之后发现了一些问题<br>
因为正在写一个vue+webpack的小项目,牵扯到热加载的问题,发现很多时候更改完代码不能执行热加载,
一开始以为是热加载配置的问题,但是用sublime打开编辑之后,发现在sublime中没有这个问题,于是查找解决办法
webstrom settings的`system settings`默认勾选`safewrite`,勾选去掉就可以了

为了方便大家,我放上图片<br>
!['图片 '图片](http://oncoxe8eo.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170424101832.png)

<p align=right>---by  lori.Wang</p>