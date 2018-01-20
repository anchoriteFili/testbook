---
layout: post
#xampp中apache不能启动处理
title:  xampp中apache不能启动处理
#时间配置
date:   2016-06-20 17:18:50 +0800
#大类配置
categories: 环境配置
#小类配置
tag: mac
---

* content
{:toc}

<a href="http://blog.sina.com.cn/s/blog_48e0ae280101hquv.html" target="_blank">mac下安装 xampp 无法启动apache</a><br>


##### 1.查看端口是否被占用 

```shell
sudo lsof -i -n
```
 
##### 2.用终端运行xampp，查看具体的错误

```shell
sudo su
/Applications/XAMPP/xamppfiles/xampp start
```
 
**多半是这个问题：**

```shell
XAMPP: Starting Apache...fail.
XAMPP:  Another web server is already running.
```
 
**解决办法：**

```shell
sudo apachectl stop
// This command killed Apache server that was pre-installed on MAC OS X.
```