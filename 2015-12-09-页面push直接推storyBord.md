---
layout: post
#标题
title: 页面push直接推storyBord
#时间配置
date:   2015-12-09 10:27:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

> 创建一个新的storybord，让页面与storybord进行连接，直接推出storybord。

##### 1. 创建一个独立的storybord

![752372-20151209110113933-2147071965.png]({{ site.img_url }}F2BC1C9366D17B55DE92794873538D27.png)

##### 2. 页面进行连接

![752372-20160510135236452-1131486059.png]({{ site.img_url }}5A1227AFB41BC2ED8AE4890A67A61E21.png)

##### 3. 用代码直接进行推

```objc
#pragma mark 类扩展进行页面的扩展
UIViewController *pushStoryboard(NSString *storyboardName,NSString *identifier) {
    
    UIStoryboard *storyBoard = [UIStoryboard storyboardWithName:storyboardName bundle:nil];
    UIViewController *viewController = [storyBoard instantiateViewControllerWithIdentifier:identifier]; //test2为viewcontroller2的StoryboardId
    return viewController;
}

/**
     如果index = 0，则利用类扩展方法进行推页面
     否则不用类扩展推页面
     */
    
    int index = 1;
    
    if (index == 0) {
        [self.navigationController pushViewController: pushStoryboard(@"Storyboard", @"test2") animated:YES];
    } else {
        UIStoryboard *storyBoard = [UIStoryboard storyboardWithName:@"Storyboard" bundle:nil];
        UIViewController *viewController = [storyBoard instantiateViewControllerWithIdentifier:@"test2"]; //test2为viewcontroller2的StoryboardId
        
        [self.navigationController pushViewController:viewController animated:YES];
    }
```