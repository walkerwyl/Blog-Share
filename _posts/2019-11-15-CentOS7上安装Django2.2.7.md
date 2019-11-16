---
layout:		post
title:		"CentOS7上安装Django2.2.7"
date:		2019-11-15
author:		"王玉松"
header-img:	""
tags:
- Django2.2.7
- CentOS7
- pyenv
---

# CentOS7上安装Django2.2.7

众所周知，Python在2.X和3.X之间存在巨大的差异，即3.X不再向后兼容2.X。这一改变的影响不仅限于Python语言本身，同时对基于Python2.X版本的第三方库也是一次挑战。在Python官网建议的直接学习Python3以及对Python2只维护到2020年的情况下，在一台笔记本电脑或服务器上同时使用Python2.X和Python3.X。
本文在阿里云的CentOS7服务器上安装Django2.2.7，数据库使用Mariadb。

## 一、安装依赖包

在Linux中，软件之间的依赖关系是十分重要的。为降低之后安装出错的可能性，必须确保软件依赖关系。

```shell
yum install gcc make git libzip-devel readline-devel zlib-devel bzip2-devel openssl openssl-devel openssl-static libffi-devel sqlite-devel mariadb-server mariadb-devel;
```

## 二、安装pyenv和pyenv-virtualenv

CentOS7上默认安装的Python版本为2.7.5，不符合Django2.2.7的开发要求。因CentOS7的yum等程序默认使用的仍然是Python2，若直接在服务器中安装Python3，即使修改了部分相关的配置文件，也无法避免可能的可能性。
因此使用pyenv创建虚拟环境与自带的Python环境完全隔离，保证其后的工作完全在Python3.7.4的环境下进行。

从Github上下载pyenv并保存在当前用户的家目录下的.pyenv目录中，向用户的配置文件.bashrc中添加命令。
pyenv-virtualenv是pyenv的插件，为pyenv添加了完整的虚拟环境功能。
在创建的新的虚拟环境中，重新定义了python命令的位置。新的python命令位置在(~/.pyenv/shim/python)，根据创建虚拟环境时指定的Python版本。之后在虚拟环境中的安装包或更新操作都只在虚拟环境中有效。

```shell
git clone git://github.com/yyuu/pyenv.git ~/.pyenv;
git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv;
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc;
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc;
echo 'eval "$(pyenv init -)"' >> ~/.bashrc;
tail .bashrc;
source ~/.bashrc;
```

下面，介绍有关pyenv的常用参数。
```text
pyenv versions	显示pyenv创建的Python虚拟环境
pyenv install 3.7.4	安装版本为3.7.4的python用于之后创建虚拟环境
pyenv uninstall 3.7.4	删除版本为3.7.4的python
pyenv virtualenv 3.7.4 venv	创建Python版本为3.7.4,名称为venv虚拟环境。具体使用时根据需要更改
pyenv active venv	激活名称为venv的虚拟环境，在激活环境之后命令提示符前会出现当前所处虚拟环境的名称。例：(venv)[root@localhost virtualenv]#
pyenv deactivate	让当前的虚拟环境失效
```

## 三、安装Django

首先为pyenv安装3.7.4的Python以便创建相应的虚拟环境，其次激活虚拟环境并在其中安装Django。在虚拟环境中的操作不会与服务器本身的Python版本产生冲突，避免了许多潜在的错误。

```shell
pyenv install 3.7.4;
pyenv virtualenv 3.7.4 venv;
cd; mkdir virtualenv; cd virtualenv;
pyenv active venv;
pip install Django; pip install mysqlclient;
```

命令执行到此，已成功安装Django2.2.7在Python版本为3.7.4的venv虚拟环境中。
相关Django的执行操作参考官方文档。

## 四、过程中出现的已解决的错误

1. pyenv install 3.7.4出现错误：
	ModuleNotFoundError: No module named '_ctypes'

	由于缺少必要的软件包libffi-devel，用yum安装即可。
```shell
yum install libffi-devel -y
```
2. python manage.py migrate进行数据库的迁移时提示一下错误：
django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
Did you install mysqlclient?

	由于缺少Python第三方软件包mysqlclient而出错。使用pip安装前，应确认是否安装mariadb-devel软件包。在缺少后者的情况下，mysqlclient是无法用pip安装的。 


3. 使用Django默认的sqlite数据库时，启动Django出现错误：
	ModuleNotFoundError: No module named '_sqlite3'
	
	由于sqlite默认安装的版本低于要求。可以到官网下载最新安装包，详见参考资源5。


4. 在软件依赖不满足的情况下，即使最后pyenv安装Python版本成功也会显示部分模块没有编译，建议是在安装上述必需的软件包之后再次重新安装Python版本。


## 参考资源
1. [Pyenv教程](https://python.freelycode.com/contribution/detail/155)
2. [CentOS 7 使用pyenv安装python3.6](https://www.cnblogs.com/afterdawn/p/9392107.html)
3. [Django文档](https://docs.djangoproject.com/en/2.2/)
4. [Centos7 安装Mariadb](https://www.cnblogs.com/yhongji/p/9783065.html)
5. [Centos 7 下升级自带 sqlite3](https://www.cnblogs.com/leffss/p/11555556.html)