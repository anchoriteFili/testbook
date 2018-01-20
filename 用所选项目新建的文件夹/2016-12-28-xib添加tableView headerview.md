---
layout: post
#xib添加tableView headerview
title:  xib添加tableView headerview
#时间配置
date:   2016-12-28 13:44:50 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

**首先在xib中拖一个UIVew进去，注意先拖入到与Controller的View并列处，如图：**

![752372-20161228134219382-1823981349.png](/styles/images/resources/9078698220F60B2E5B042C146883B41B.png)<br>

**然后将，这个View拖到File’s Ower 的那一并列层中，（按照箭头所指的方向拖拽即可）如图：**

![752372-20161228134238664-272762510.png](/styles/images/resources/BCB3D4B334835CA7EE8152509B3F35AA.png)<br>
![752372-20161228134254179-1523045870.png](/styles/images/resources/056367D6988656591DFE4BA8FEDD22CF.png)<br><br>

**接下来的一步不要忘记：把刚刚的UIView的Simulated Metrics的Size设置成Freeform**
**(忘记此步骤下面就不成功了)，然后再将这个VIew拖入到TableView，还是按图箭头方向操作即可，如图:**

![752372-20161228134303382-1451174517.png](/styles/images/resources/B68051D4219BEBDC59644CC8C0F184A3.png)<br>
![752372-20161228134319539-252709669.png](/styles/images/resources/2B484344192FC4CB2E6D95A803B0D1DD.png)