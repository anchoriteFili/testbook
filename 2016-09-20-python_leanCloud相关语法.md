---
layout: post
#python_leanCloud相关语法
title:  python_leanCloud相关语法
#时间配置
date:   2016-09-20 18:04:50 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}

**相关链接：**<br>

* <a href="https://leancloud.cn/docs/sdk_setup-python.html#Python_SDK_安装指南" target="_blank">Python SDK 安装指南</a><br>

**pip 是最推荐的 Python 包管理工具，它是 easy_install 的替代。安装 leancloud-sdk 只需执行以下命令：**

```shell
$ pip install leancloud-sdk
```

**也可以使用 easy_install 进行安装：**

```
easy_install leancloud-sdk
```

**导入头文件，并初始化**

```py
import leancloud

leancloud.init("eb5QvTbUUq06b0qvyLESRa5Y", "iu2j1fbxBOJirqHc")
```
**保存功能的实现**

```py
def leanCloudSave(MovieName, MovieUrl, MovieImageUrl):
    Todo = leancloud.Object.extend('MyselfMovie')
    todo = Todo()
    todo.set('MovieImageUrl', MovieImageUrl)
    todo.set('MovieUrl', MovieUrl)
    todo.set('MovieName', MovieName)  # 增加一个字段
    todo.save()
```

**查询功能的实现**

```py
Todo = leancloud.Object.extend('KTVMusic')
query = Todo.query
query.select('MusicName','MusicUrl','MusicSinger')
query.limit(10)
query_list = query.find()

for todo in query_list:
    title = todo.get('MusicName')
    content = todo.get('MusicUrl')
    # 如果访问没有指定返回的属性（key），则会返回 null
    location = todo.get('MusicSinger')
    print title, content, location
```


 