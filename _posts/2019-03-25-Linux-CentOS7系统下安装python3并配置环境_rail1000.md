#        Linux-CentOS7系统下安装python3并配置环境

---
layout:     post
title:      "第四周python机器学习预测分析学习笔记"
date:       2019-03-28
author:     "桂志强"
header-img: "img/post-bg-2015.jpg"
tags:
    - python
    - 学习笔记
---



首先，因为centos7以上的系统已经安装了python2，所以，如果直接安装python3的话，之后会出现一系列的问题，所以需要修改环境

输入命令python -V查看环境:

`python -V`

发现是python２.7.5，这个python版本已经不适合现在的开发了，但是centos系统的yum又是依赖python2.7.5的，所以需要解决这个问题

在安装python3之前需要安装相关包，命令如下：

``yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make``

开始编译安装python3，去官网下载编译安装包，地址是：https://www.python.org/ftp/python/3.6.2/

解压之后进入文件夹

`cd Python-3.6.2`

开始编译安装：

`./configure prefix=/usr/local/python3`

`make && make install`

安装完毕，添加软链：

`ln -s /usr/local/python3/bin/python3 /usr/bin/python`

之后查看python版本：

`python -V`

查看python2版本：

`python2 -V`

然后因为yum需要python2，所以还要修改：

`vim /usr/bin/yum`

把文件开头的#! /usr/bin/python修改为#! /usr/bin/python2

然后：

`vim /usr/libexec/urlgrabber-ext-down`

#! /usr/bin/python 也修改为#! /usr/bin/python2

之后python3就算安装成功了
