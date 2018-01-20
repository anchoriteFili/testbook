---
layout: post
#标题
title:  python_leancloud福利相关
#时间配置
date:   2016-09-20 17:53:12 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}


```py
#!/usr/bin/python
#-*- coding: utf-8 -*-
#encoding=utf-8

import requests
import htmllib
import re
import urllib2
import json
import leancloud
from lxml import etree
import sys
reload(sys)
sys.setdefaultencoding('utf8')

def savefile(string):
    f = open("/Users/macbook/python/代码/电影名单.txt","a")
    f.writelines(string)
    f.close()

def getFilmWithPage(page):
    URL = 'http://hehe/list3/%d.html' % page

    html1 = requests.get(URL, 'GET')
    # print html1.text
    pythonEtree = etree.HTML(html1.text)
    pythonLink = pythonEtree.xpath('//div[@class="video_box"]/a')

    # film = ''
    for each in pythonLink:
        # print each.xpath('img/@src')[0]
        # print each.xpath('img/@title')[0]
        # print each.xpath('@href')[0]
        searchObjOne = re.search(r'/vod/(.*?).html', each.xpath('@href')[0], re.M | re.I | re.S)
        # print searchObjOne.group(1)

        URLTwo = 'http://www.hehe.co/jwplayer/jwconfig.php?vkey=%s.html' % searchObjOne.group(1)
        # print URLTwo
        html2 = requests.get(URLTwo, 'GET')
        # print html2.text
        pythonEtree2 = etree.HTML(html2.text)
        pythonLink2 = pythonEtree2.xpath('//config/file/text()')

        leanCloudSave(each.xpath('img/@title')[0], pythonLink2[0], each.xpath('img/@src')[0])


        # print pythonLink2[0]
        # film += '{\n\"电影名字\":\"%s\",\n\"电影连接\":\"%s\",\n\"图片链接\":\"%s\"\n},\n' % (
        # each.xpath('img/@title')[0], pythonLink2[0], each.xpath('img/@src')[0])
    print page
    # print film
    # savefile(film)

def leanCloudSave(MovieName, MovieUrl, MovieImageUrl):
    Todo = leancloud.Object.extend('MyselfMovie')
    todo = Todo()
    todo.set('MovieImageUrl', MovieImageUrl)
    todo.set('MovieUrl', MovieUrl)
    todo.set('MovieName', MovieName)  # 增加一个字段
    todo.save()

# 初始化
leancloud.init("eb5QvTbUUq06b0qvyLE", "iu2j1fbxBOJi")

for num in range(19, 30):
    getFilmWithPage(num)
```