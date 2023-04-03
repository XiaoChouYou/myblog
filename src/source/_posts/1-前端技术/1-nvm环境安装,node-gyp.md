---
title: 1-nvm环境安装,node-gyp安装
date: 2023-02-11 15:15:31
categories:
  - [日常工作经验记录,前端,搭建个人博客,环境准备]
tags:
  - nvm
  - node
---



## 一、背景介绍：node-gyp是干啥用的~
node-gyp，是由于node程序中需要调用一些其他语言编写的 工具 甚至是dll，需要先编译一下，否则就会有跨平台的问题，例如在windows上运行的软件copy到mac上就不能用了，但是如果源码支持，编译一下，在mac上还是可以用的。node-gyp在较新的Node版本中都是自带的（平台相关），用来编译原生C++模块。
## 二、在一个新的vue项目中安装：
先在控制台输入：npm install --global --production windows-build-tools（此命令为一键安装）"********一定注意 在Windows powerShell（管理员）输入指令****"
为啥要一键安装呢，安装的是啥呢？
解释：

* 1、python(v2.7 ，3.x不支持);
* 2、visual C++ Build Tools
* 3、.net framework 4.5.1
就是安装的这三个东西，安装时间有点长，别着急，慢慢等~

然后在控制台输入：npm install -g node-gyp
【只需两部就安装好了】
## 三、注意点：
在node-gyp安装前，一定是有node.js的，而且一定是32位的，如果你电脑是windows64位的，并且安装了64位的node.js,[请阅读](https://www.cnblogs.com/wangyuxue/p/11217889.html)
## 四、安装完成后查看：
控制台输入：node-gyp list

