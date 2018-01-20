---
layout: post
#标题
title:  在window上方添加view
#时间配置
date:   2016-04-28 15:05:22 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![752372-20160428151328033-169563347.png]({{ site.img_url }}24040DFD0265171F557D5E6A5234046C.png)

**特别特别要注意的东西**

> 1. 在appdelegate中,所有的层次都要添加到[self.window makeKeyAndVisible]的后面，否则创建的任何view都会在rootViewController的后面

```objc
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    [self.window makeKeyAndVisible];
    self.window.rootViewController = [[BlueViewController alloc] init];
    [self setStartAnamation];
```

> 2. 在VC中添加window上的view，这个view的添加事件只能在点击事件等页面显示过后的事件中，如果添加到viewDidLoad等显示方法中，view会显示到VC的后方

 