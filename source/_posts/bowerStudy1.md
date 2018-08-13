---
title: bower入门(一)
date: 2017-03-30 21:52
categories: bower
tag: ['bower']
---
## 一.准备
- 安装node环境:node.js
- 安装Git，bower从远程git仓库获取代码包
## 二.安装bower
使用npm，打开终端，输入：
```javascript
npm install -g bower
```
## 三.包的安装
类似于npm<br>
如:我要安装一个jquery
```javascript
bower install jquery --save
```
如果不是在git的命令行输入的话  会提示报错<br>
```
bower  ENOGIT git is not installed or not in the PATH
```
此时需要确定你是否安装了git<br>
然后在该目录下打开git bash  重新安装

## 四.安装指定版本的包
与`npm`不同的是`bower`使用`#`来指定版本
```javascript
bower install jquery#2.1.2
```
## 五.查看包的信息
可以查看仓库中包的版本号和包的信息
```javascript
bower info angular
```
出现的数据如下,为了方便我就不上传图片了,直接放到代码框里
```javascript
{
  name: angularjs,//包的名字
  version: '1.6.3',//包最新的版本
  license: 'MIT',
  main: angularjs,
  ignore: [],
  dependencies: {},
  homepage: angularjs//包的地址
}
//可以用的版本号
Available versions:
  - 1.6.3
  - 1.6.2
  - 1.6.1
  - 1.6.0
  - 1.5.11
  - 1.5.10
  - 1.5.9
  - 1.5.8
  - 1.5.7
  - 1.5.6
  - 1.5.5
  - 1.5.4
  - 1.5.3
  - 1.5.2
  - 1.5.1
  - 1.5.0
  - 1.4.14
  - 1.4.13
  - 1.4.12
  - 1.4.11
  - 1.4.10
  - 1.4.9
  - 1.4.8
  - 1.4.7
  - 1.4.6
  - 1.4.5
  - 1.4.4
  - 1.4.3
  - 1.4.2
  - 1.4.1
  - 1.4.0
  - 1.3.20
  - 1.3.19
  - 1.3.18
  - 1.3.17
  - 1.3.16
  - 1.3.15
  - 1.3.14
  - 1.3.13
  - 1.3.12
  - 1.3.11
  - 1.3.10
  - 1.3.9
  - 1.3.8
  - 1.3.7
  - 1.3.6
  - 1.3.5
  - 1.3.4
  - 1.3.3
  - 1.3.2
  - 1.3.1
  - 1.3.0
  - 1.2.32
  - 1.2.31
  - 1.2.30
  - 1.2.29
  - 1.2.28
  - 1.2.27
  - 1.2.26
  - 1.2.25
  - 1.2.24
  - 1.2.23
  - 1.2.22
  - 1.2.21
  - 1.2.20
  - 1.2.19
  - 1.2.18
  - 1.2.17
  - 1.2.16
  - 1.2.15
  - 1.2.14
  - 1.2.13
  - 1.2.12
  - 1.2.11
  - 1.2.10
  - 1.2.9
  - 1.2.8
  - 1.2.7
  - 1.2.6
  - 1.2.5
  - 1.2.4
  - 1.2.3
  - 1.2.2
  - 1.2.1
  - 1.2.0
  - 1.0.8
  - 1.0.7
  - 1.0.6
  - 1.0.5
  - 1.0.4
```
## 六.包的查找
一个很重要的功能,包的查找，比如我想要安装`bootstrap`的某个插件，但是记不住名字了，就可以直接在命令行输入
```
bower search bootstrap
```
`bower`就会列出包含字符串`bootstrap`的可用包了
## 七.卸载包
```
bower uninstall jquery
```
## 八.包的初始化
本来这个东西应该放到最开始,但是我用bower只是要文件,所以不常用= - =<br.
类似于`npm init`<br>
```
bower init
```
生成一个`bower.json`文件，用来保存该项目的配置，如下：
```json
{
  "name": "bb_boot",
  "version": "0.0.1",
  "authors": [
    "savokiss <jaynaruto@qq.com>"
  ],
  "moduleType": [
    "amd"
  ],
  "license": "MIT",
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "js/lib",
    "test",
    "tests"
  ],
  "dependencies": {
  }
}
```