---
layout: post
#标题
title:  获取当前正在显示的viewController
#时间配置
date:   2015-10-16 13:08:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


<a href="http://blog.csdn.net/worldzhy/article/details/42120929" target="_blank">iOS 获取当前正在显示的ViewController</a><br>

```objc
//获取当前屏幕显示的viewcontroller
- (UIViewController *)getCurrentVC
{
    UIViewController *result = nil;
    
    UIWindow * window = [[UIApplication sharedApplication] keyWindow];
    if (window.windowLevel != UIWindowLevelNormal)
    {
        NSArray *windows = [[UIApplication sharedApplication] windows];
        for(UIWindow * tmpWin in windows)
        {
            if (tmpWin.windowLevel == UIWindowLevelNormal)
            {
                window = tmpWin;
                break;
            }
        }
    }
    
    UIView *frontView = [[window subviews] objectAtIndex:0];
    id nextResponder = [frontView nextResponder];
    
    if ([nextResponder isKindOfClass:[UIViewController class]])
        result = nextResponder;
    else
        result = window.rootViewController;
    
    return result;
}
```