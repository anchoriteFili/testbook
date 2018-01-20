---
layout: post
#标题
title:  替换UIViewController中viewWillAppear和viewWillDisappear类目
#时间配置
date:   2016-05-19 13:13:22 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
#import <UIKit/UIKit.h>

@interface UIViewController (RunTimeRewriteMessage)

@end

#import "UIViewController+RunTimeRewriteMessage.h"
#import <objc/runtime.h>
#import "MobClick.h"
@implementation UIViewController (RunTimeRewriteMessage)

+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [self class];
        exchangeMethod(class, @selector(viewWillAppear:), @selector(APP_viewWillAppear:));
        exchangeMethod(class, @selector(viewWillDisappear:), @selector(APP_viewWillDisappear:));
    });
}
void exchangeMethod(Class class, SEL theMethodSelector, SEL changeMethodSelector){
    Method theMethod = class_getInstanceMethod(class, theMethodSelector);
    Method changeMethod = class_getInstanceMethod(class, changeMethodSelector);
    method_exchangeImplementations(theMethod, changeMethod);
    // 如果 exChange 的是类方法, 采用如下的方式:
    // Class class = object_getClass((id)self);
    // ...
    // Method originalMethod = class_getClassMethod(class, originalSelector);
    // Method swizzledMethod = class_getClassMethod(class, swizzledSelector);
}
#pragma mark - Method Swizzling
//
- (void)APP_viewWillAppear:(BOOL)animated {
    //看似是自己调用自己的方法， 但是实质上却是调用交换后的viewWillAppear，不会引起死循环；
    [self APP_viewWillAppear:animated];
    if ([self isKindOfClass:[UIAlertController class]]) {
            NSLog(@"333333");
    }
    NSString * className = [NSString stringWithUTF8String:object_getClassName(self)];
    [MobClick beginLogPageView:className];
    NSLog(@"APP_viewWillAppear: %@", self);
}
- (void)APP_viewWillDisappear:(BOOL)animated{
    [self APP_viewWillDisappear:animated];
    NSString * className = [NSString stringWithUTF8String:object_getClassName(self)];
    [MobClick  endLogPageView:className];
    NSLog(@"APP_viewWillDisappear: %@", self);
}

@end
```