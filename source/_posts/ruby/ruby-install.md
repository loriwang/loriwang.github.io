---
title: Ruby入门
date: 2017-07-22 20:56
categories: ['ruby']
type: ['ruby']
---
# Ruby入门
`ruby`是一门脚本语言,最近因为公司的业务需要,需要`redmine`上进行二次开发,以满足需求,所以开始学习ruby了,首先我们要做的事情是对环境的安装.
## 1.环境的安装
因为自己使用的是`windows`系统,所以下面只介绍`windows`系统的环境的安装.<br>
在`windows`系统下面我们使用的是`ruby-installer`进行的安装下载地址为[下载地址](https://rubyinstaller.org/downloads/),选择`RubyInstallers`进行下载
## 2.RubyInstallers 的下载
注意上面使用的地址是外网的地址所以朋友们如果当无法下载的时候需要找个翻墙软件进行下载,我在公司使用的专门的外网环境,在家使用的是`蓝灯`工具,[下载地址](http://pan.baidu.com/s/1kVyuYt5)已经贴上,大家可以上去找各个版本的工具进行下载.<br>
不过需要注意的是,免费用户一个月只有`500MB`的流量可以使用,所以大家在使用的时候注意一下流量的控制.

## 3.RubyInstallers 的安装
下载完了软件的时候我们就按照正常的流程进行软件的安装就行了.<br>
不过.在进行选项的选择的时候`把ruby执行文件设置到环境变量PATH`勾选上.如图:
!['图片' '图片'](http://hi.csdn.net/attachment/201102/24/0_129852361586R1.gif)
## 4.安装完成的测试
然后我们就可以在`windows`的开始菜单中选择`Start Command Prompt with Ruby`打开`Ruby`的命令行.<br>
输入`ruby -v`测试是否能够正常输出Ruby的当前版本.<br>
如果勾选了 上面的添加环境变量选项的话,直接打开命令行就行了.

## 5.Rubymine上命令行环境
因为我是先安装的`Rubymine`后安装的ruby环境,所以在`rubymine`上的命令行上,刚开始安装上并没有`ruby`的环境,重启下`Rubymine`即可解决
<p align=right>---by  lori.Wang</p>