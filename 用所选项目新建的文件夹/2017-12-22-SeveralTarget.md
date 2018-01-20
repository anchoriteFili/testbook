---
layout: post
#一个项目多个target的创建
title: 一个项目多个target的创建
#时间配置
date:   2017-12-22 14:44:00 +0800
#大类配置
categories: xcode功能
#小类配置
tag: xcode
---

* content
{:toc}

### 引用链接
---

* [iOS同一项目多个Target的快速实现方法 - 两种使用场景详解](http://www.cnblogs.com/yajunLi/p/6001132.html)

### 场景一： 一个项目配置多个环境
-------------------

##### 1> 在tagets中直接添加Duplicate

<img src="/styles/images/2017-12-22-SeveralTarget/1.png" width = "80%" />

##### 2> 添加target并重新命名为runtimeOC

<img src="/styles/images/2017-12-22-SeveralTarget/2.png" width = "80%" />

##### 3> 修改Schemes中的名字

<img src="/styles/images/2017-12-22-SeveralTarget/3.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/4.png" width = "80%" />

##### 4> 将自动生成的plist重新命名 -> 删除 -> 重新加载进入标记

<img src="/styles/images/2017-12-22-SeveralTarget/5.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/6.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/7.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/8.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/9.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/10.png" width = "80%" />

##### 5> 在项目中定义特定的宏作为区分（OC用macros，swift用swift_flag）

<img src="/styles/images/2017-12-22-SeveralTarget/11.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/12.png" width = "80%" />

> swift在swift_flag中添加宏，并且宏必须是字符串，每个宏都要用"-D"隔开，否则失效

<img src="/styles/images/2017-12-22-SeveralTarget/13.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/14.png" width = "80%" />

### 场景二： 项目中直接创建一个整体项目target(相当于另外一个项目)
-------------------
<img src="/styles/images/2017-12-22-SeveralTarget/15.png" width = "80%" />
<img src="/styles/images/2017-12-22-SeveralTarget/16.png" width = "80%" />
