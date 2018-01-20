---
layout: post
#标题
title:  python获取cookie，抓取页面数据
#时间配置
date:   2017-05-19 15:56:12 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}

**打开火狐浏览器，直接用开发工具进行获取**

![752372-20170519153515275-1785180501.png]({{ site.img_url }}D8C92F987AC06C2179653D706E95CFAC.png)

**抓取页面数据**

```py
#!/usr/bin/python
#-*- coding: utf-8 -*-
#encoding=utf-8

import hashlib
import time
import sys
import base64
import requests
import json
reload(sys)
import re
from lxml import etree
sys.setdefaultencoding('utf8')

# 添加header,获取通行证
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:52.0) Gecko/20100101 Firefox/52.0',
           'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
           'Cookie': 'pgv_pvi=2113857536; pt2gguin=o0243097674; RK=sc0TQxoWQY; ptcz=641c39ca965688bd9a25fdca237c8c99613a7ef334b7677582662ce841df7d45; uin=o0243097674; skey=MUPUgOZjd2; zzpaneluin=; zzpanelkey=; p_skey=ZFqBKiWsl09*i2oeY7URuWnpOF60wyc4tc6MxiVJxBI_; pt4_token=Ovj0Px026GMQ4pF-*bjWCHEuIMTk9YsEuHO3Qky2iSI_; p_uin=o0243097674; pgv_si=s8164508672; ptisp=cnc; Loading=Yes; pgv_pvid=490904736; pgv_info=ssid=s6547105191; QZ_FE_WEBP_SUPPORT=0; qz_screen=1280x800; cpu_performance_v8=52; __Q_w_s__QZN_TodoMsgCnt=1; __Q_w_s__appDataSeed=1; g_ut=2',
           }

# 开始抓取数据
url = 'https://user.qzone.qq.com/243097674/'
html = requests.get(url,headers=headers)
print html.text
```