---
layout: post
#标题
title:  UITabBarController/UINavigationController/UIViewController的区别与联系
#时间配置
date:   2015-10-21 11:21:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


**相同点：**

> 首先，三者同属于UIViewController，那么这就有了共同点，那就是三者都能作为window的RootViewController,即<self.window.rootViewController=>，其中UIViewController是最基本的单元，单独的一个单元，而前两者是比较复杂的两个ViewController

**不同点：**

> 1. 空间不同，ViewController只是一个基本单元，只显示一个页面，自己没有跳转功能。UITabBarController这个是一个横向的结构，就是把一堆的ViewController集中到自己这一个平面上，横向平面，下面的一个一个的小块代表的就是一个一个的页面，而第二者UINavigationController是纵向的结构，也是领导者一堆的页面，但是是纵向的，一层一层的进行推进
>
> 2. 表现形式不同，ViewController只是一个单独页面，而UITabBarController是一个横向的页面，UITabBarController就是一个载体，承载着多个互不相干的ViewController，将这些ViewController集中在下面的小模块里触发点击事件，UINavigationController则不同，每次只显示一张页面，然后依次递推，一页一页的递推，是一串的相互关联的页面。

> 三者的本质是一个样的，不过UITabBarController相当于一个数组，有着一并排的ViewController,而第二者UINavigationController相当于一个字典，一个套一个，一个套一个的走着。