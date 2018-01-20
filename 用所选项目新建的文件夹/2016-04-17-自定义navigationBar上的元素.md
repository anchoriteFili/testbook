---
layout: post
#标题
title: 自定义navigationBar上的元素
#时间配置
date:   2016-04-17 00:40:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
--- 

* content
{:toc}

```objc
if ([UIDevice currentDevice].systemVersion.floatValue >= 7.0) {
        //[[UINavigationBar appearance] setBarTintColor:RGBACOLOR(78, 188, 211, 1)];
        [[UINavigationBar appearance] setTintColor:RGB_COLOR(112, 182, 35)];
        
        [[UINavigationBar appearance] setTitleTextAttributes:
         [NSDictionary dictionaryWithObjectsAndKeys:RGB_COLOR(112, 182, 35), NSForegroundColorAttributeName, [UIFont fontWithName:@ "HelveticaNeue-CondensedBlack" size:18.0], NSFontAttributeName, nil]]; //设置字体颜色等
//        [UINavigationBar appearance].backItem.leftBarButtonItem.image = [UIImage imageNamed:@"绿返回按钮"];
        [[UIBarButtonItem appearance] setBackButtonTitlePositionAdjustment:UIOffsetMake(0, -60) forBarMetrics:UIBarMetricsDefault]; //去掉返回按钮的文字
    }
```

**swift设置**

```swift
//设置全局导航栏的各种属性
        UINavigationBar.appearance().tintColor = UIColor.whiteColor()
        UINavigationBar.appearance().barTintColor = UIColor(red: 231.0/255.0, green: 95.0/255.0, blue: 53.0/255.0, alpha: 0.3) //修改导航栏背景色
        let textAttrs2 = NSDictionary(objects: [UIColor.whiteColor(),UIFont.systemFontOfSize(20)], forKeys: [NSForegroundColorAttributeName,NSFontAttributeName])
        UINavigationBar.appearance().titleTextAttributes = (textAttrs2 as! [String : AnyObject]) //为导航栏设置字体颜色等
```