---
title: angular-ui-bootstrap分页
date: 2017-04-01 21:52
categories: angularJs
tag: ['angularJs','bootstrap','angular-ui-bootstrap']
---
# 一.需要安装的东西
- angularJs1  我用的版本是1.5.3,官方建议1.4.x
- bootstrap.css  官方建议是 2.3.x以上 我用的是3.3.x
- angular-ui-bootstrap [下载地址]("https://angular-ui.github.io/bootstrap/"),我用的版本是2.5.0
# 二.注入
```javascript
angular.module('myModule', ['ui.bootstrap']);
```
# 三.分页的标签
```html
    <ul uib-pagination
        class="pagination-lg"
        total-items="bigTotalItems"
        ng-model="CurrentPage"
        ng-change="pageChanged(CurrentPage)"
        max-size="maxSize"
        boundary-link-numbers="false"
        boundary-links="true"
        rotate="true"
        force-ellipses="true"
        last-text="最后一页"
        first-text="第一页"
        next-text="下一页"
        previous-text="上一页"
    ></ul>
```
说明
- `ng-model` 绑定页码值;可以设定刚打开时的页码值,也会触发`ng-change`函数
- `ng-change` 页码改变时触发的函数
- `total-items` 总共多少条数据
- `max-size`  最多显示多少个页码,即超出多少页码多余的页码会隐藏
- `previous-text` 上一页的文字,默认`Previous`
- `next-text` 下一页的文字,默认`Next`
- `first-text` 第一页的文字,默认`First`
- `last-text` 第一页的文字,默认`Last`
- `force-ellipses` 设定`rotate`为`true`;且当前页少于最大页时候 会预读下面的页数
- `rotate`  当前页是否一直可见