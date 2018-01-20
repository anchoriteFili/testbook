---
layout: post
#标题
title:  xib中添加UIScrollView
#时间配置
date:   2016-05-09 11:55:22 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://small.qiang.blog.163.com/blog/static/978493072015292522113/" target="_blank">Storyboard、xib中的UIScrollView使用autolayout，使其能够滚动</a><br>

##### 添加的步骤：

> 1. 拖拽scrollView
> 
> 2. 添加scrollView的四周限制
> 
> 3. 拖拽一个View添加到scrollView
> 
> 4. 添加view的四周限制(现在爆红不要管)
> 
> 5. 竖屏滚动添加竖直居中限制/水平滚动添加横屏居中限制
> 
> 6. 竖屏滚动添加view固定高度并拉出属性/水平滚动添加view的固定宽度并拉出属性
> 
> 7. 此时爆红消失