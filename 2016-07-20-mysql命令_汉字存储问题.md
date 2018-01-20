---
layout: post
#mysql命令_汉字存储问题 
title:  mysql命令_汉字存储问题 
#时间配置
date:   2016-07-20 11:07:50 +0800
#大类配置
categories: 环境配置
#小类配置
tag: mysql
---

* content
{:toc}

### 相关链接
---


* <a href="http://www.2cto.com/database/201308/236961.html" target="_blank">解决mysql的汉字问题</a><br>
* <a href="http://www.cnblogs.com/xdpxyxy/archive/2012/11/16/2773662.html" target="_blank">Linux下MySQL数据库常用基本操作 一</a><br>
* <a href="http://www.jb51.net/article/55849.htm" target="_blank">MySQL存储引擎总结</a><br>

### 本地mysql库的设置
---

##### 1. 找到my.cnf文件

![752372-20160720103948701-1568184487.png]({{ site.img_url }}68A1E7717A4BFB58DB29396B275424F8.png)

##### 2. 打开my.cnf

**添加相关内容**

```
[mysqld]
character-set-server=utf8
[mysql]
default-character-set=utf8
```

##### 3. 重启mysql

![752372-20160720104429326-574223340.png]({{ site.img_url }}8C48438AEC85F9F98707D5C1BB70479A.png)