---
layout: post
#python添加mysql工具MySQLdb相关步骤
title:  python添加mysql工具MySQLdb相关步骤
#时间配置
date:   2016-07-26 14:12:50 +0800
#大类配置
categories: 环境配置
#小类配置
tag: python
---

* content
{:toc}

### 相关链接
---

* <a href="http://blog.csdn.net/janronehoo/article/details/25207825" target="_blank">在 Mac 中安装 MySQLdb (Python mysql )</a><br>
* <a href="http://www.52codes.net/article/1003.html" target="_blank">安装MYSQL-PYTHON包报错mysql_config not found解决办法</a><br>


##### 1. 下载MySQLdb的压缩包（<a href="https://sourceforge.net/p/mysql-python/code/ci/master/tree/" target="_blank">MySQLdb</a>）

![752372-20160725175035606-74336341.png]({{ site.img_url }}EE4B650AC4257C60E51C2DF5526CADCC.png)

##### 2. 修改解压文件中site.cfg中mysql_config的相关路径，用的是xampp中的路径

```py
[options]
# embedded: link against the embedded server library
# threadsafe: use the threadsafe client
# static: link against a static library (probably required for embedded)

embedded = False
threadsafe = True
static = False

# The path to mysql_config.
# Only use this if mysql_config is not on your PATH, or you have some weird
# setup that requires it.
mysql_config = /Applications/XAMPP/xamppfiles/bin/mysql_config 

# http://stackoverflow.com/questions/1972259/mysql-python-install-problem-using-virtualenv-windows-pip
# Windows connector libs for MySQL. You need a 32-bit connector for your 32-bit Python build.
connector = C:\Program Files (x86)\MySQL\MySQL Connector C 6.0.2
```

##### 3. 终端中运行命令

```shell
$ cd /Users/macbook/Downloads/mysql-python-code-7676693b8f5f1b9ae62f40f4c872fc1e3777ba71/MySQLdb 
$ python setup.py install
```

##### 4. 此时会出现`'my_config.h' file not found`的错误

> 这是因为MAMP自带的MySQL不包含dev headers，使用brew install mysql-connector-c安装。

![752372-20160725180004028-1133905084.png]({{ site.img_url }}41CD76D80BDE70F8D716888DBDC03412.png)

##### 5. 再次运行 `$ python setup.py install` 命令，运行成功

![752372-20160725180204981-810936018.png]({{ site.img_url }}BA2D1EA7DC02B1ABC892A70AA2A4B59C.png)