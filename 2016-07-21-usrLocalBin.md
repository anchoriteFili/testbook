---
layout: post
#/usr/local/bin is not in your PATH. 解决方案
title:  /usr/local/bin is not in your PATH. 解决方案
#时间配置
date:   2016-07-21 15:12:50 +0800
#大类配置
categories: 环境配置
#小类配置
tag: mac
---

* content
{:toc}

**两个命令**

```shell
$ export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin/:/sbin:/bin:/usr/game:$PATH
终端执行命令 关闭终端，效果就会消失
$ export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
永久的添加命令，这个是好东西
```