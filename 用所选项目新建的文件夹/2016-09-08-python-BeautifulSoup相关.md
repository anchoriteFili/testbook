---
layout: post
#python-BeautifulSoup相关
title:  python-BeautifulSoup相关
#时间配置
date:   2016-09-08 20:56:50 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}

**相关链接**<br>

* <a href="http://cuiqingcai.com/1319.html" target="_blank">Python爬虫利器二之Beautiful Soup的用法</a><br>

**安装相关**<br>

```shell
pip install beautifulsoup4
```

**如果报警告，获取管理员权限**

```shell
sudo pip install beautifulsoup4
```

**相关示例：**

```py
#!/usr/bin/env python
# -*- coding:utf-8 -*-

import urllib
from bs4 import BeautifulSoup
import fnmatch
import sys

if __name__ == '__main__':
    # url = sys.argv[1] argv[1]可以从终端获取第一个参数,这个参数是个链接
    url = "http://image.baidu.com/search/detail?ct=503316480&z=0&ipn=d&word" \
          "=%E5%9B%BE%E7%89%87%E5%A4%A7%E5%85%A8&step_word=&hs=0&pn=7&spn=0&di=17" \
          "0858495280&pi=&rn=1&tn=baiduimagedetail&is=&istype=0&ie=utf-8&oe=utf-8&in" \
          "=&cl=2&lm=-1&st=undefined&cs=771442869%2C3734549669&" \
          "os=4146841493%2C2140196118&simid=3454161402%2C234964101&adpicid=0&ln=1982&" \
          "fr=&fmq=1473337294927_R&fm=&ic=undefined&s=undefined&se=&sme=&tab=0&" \
          "width=&height=&face=undefined&ist=&jit=&cg=&bdtype=0&oriquery=&objurl=http" \
          "%3A%2F%2Fpic250.quanjing.com%2FFLI_VC3%2FFV3518.jpg&fromurl=ippr_z2C%" \
          "24qAzdH3FAzdH3Fktzitpjfp_z%26e3Bq7wg3tg2_z%26e3Bv54AzdH3Fp5rtvAzdH3FFVncb" \
          "a&gsm=0&rpstart=0&rpnum=0"
    html = urllib.urlopen(url).read() # 获取链接内容
    soup = BeautifulSoup(html, "lxml") # 添加BeautifulSoup过滤参数

    for link in soup.find_all('img'): # 找到所有img标签的内容
        content = link.get("src") # 找到img中的属性src内容
        if type(content) == type(None): # 如果内容为空,直接跳过
            pass
        elif fnmatch.fnmatch(content,"*.jpg"): # 如果内容为jpg格式则输出
            print content
        else: # 其他一切都跳过
            pass
```