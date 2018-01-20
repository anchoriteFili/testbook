---
layout: post
#标题
title:  隐藏tabBar和navigationbar
#时间配置
date:   2015-05-30 21:29:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


#### 隐藏tabbar有两种方式

##### 1：在pushViewController之前调用

```objc
[self setHidesBottomBarWhenPushed:YES]; 
```

**同时在viewWillDisappear调用：**

```objc
- (void)viewWillDisappear:(BOOL)animated {  
    [self setHidesBottomBarWhenPushed:NO];  
    [super viewDidDisappear:animated];  
}
```
 

##### 2：使用函数：

```objc
- (void) hideTabBar:(BOOL) hidden{  
    [UIView beginAnimations:nil context:NULL];  
    [UIView setAnimationDuration:0];  
    for(UIView *view in self.tabBarController.view.subviews)  
    {  
        if([view isKindOfClass:[UITabBar class]])  
        {  
            if (hidden) {  
                [view setFrame:CGRectMake(view.frame.origin.x, 480, view.frame.size.width, view.frame.size.height)];  
            } else {  
                [view setFrame:CGRectMake(view.frame.origin.x, 433, view.frame.size.width, view.frame.size.height)];  
            }  
        }  
        else  
        {  
            if (hidden) {  
                [view setFrame:CGRectMake(view.frame.origin.x, view.frame.origin.y, view.frame.size.width, 480)];  
            } else {  
                [view setFrame:CGRectMake(view.frame.origin.x, view.frame.origin.y, view.frame.size.width, 433)];  
            }  
        }  
    }  
    [UIView commitAnimations];  
}
```
> 方式一的方法代码简单，且界面也好控制一些


#### 如何隐藏UINavigationBar

**显示：**

```objc
[self.navigationController setNavigationBarHidden:NO animated:YES];
```
**隐藏：**

```objc
[self.navigationController setNavigationBarHidden:YES animated:YES];
```

**隐藏返回键**

```objc
self.navigationItem.hidesBackButton = YES;
```