---
layout: post
#标题
title:  自定义navigation宏，三原色宏和获取屏幕宽高度的宏
#时间配置
date:   2015-09-07 16:31:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/Navigation.h.zip" target="_blank">Navigation.h.zip</a><br>

```objc
#ifndef test8photos_Header_h
#define test8photos_Header_h

#pragma mark 颜色转换宏
#define RGBA_COLOR(R, G, B, A) [UIColor colorWithRed:((R)/255.0f) green:((G)/255.0f) blue:((B)/255.0f) alpha:A]
#define RGB_COLOR(R, G, B) [UIColor colorWithRed:((R)/255.0f) green:((G)/255.0f) blue:((B)/255.0f) alpha:1.0f]

//比例的适配
#define WIDTH [UIScreen mainScreen].bounds.size.width
#define HEIGHT [UIScreen mainScreen].bounds.size.height
#define WIDTHADP [UIScreen mainScreen].bounds.size.width/375.0
#define HEIGHTADP [UIScreen mainScreen].bounds.size.height/667.0
#define GETX(VIEW) (VIEW).frame.origin.x
#define GETY(VIEW) (VIEW).frame.origin.y
#define GETWIDTH(VIEW) (VIEW).frame.size.width
#define GETHEIGHT(VIEW) (VIEW).frame.size.height

//leftBarButtonItem自定义
#define LEFTBARBTN(imgName,method)\
UIImage *image = [[UIImage imageNamed:imgName] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];\
UIBarButtonItem *leftButton = [[UIBarButtonItem alloc ] initWithImage:image style:UIBarButtonItemStylePlain target:self action:@selector(method)];\
self.navigationItem.leftBarButtonItem = leftButton;

//navigation中间title自定义
#define NAVTITLE(title)\
UILabel *label = [[UILabel alloc] initWithFrame:CGRectZero];\
label.backgroundColor = [UIColor clearColor];\
label.font = [UIFont systemFontOfSize:18];\
label.textAlignment = NSTextAlignmentCenter;\
label.textColor = RGB_COLOR(112, 182, 35);\
self.navigationItem.titleView = label;\
label.text = title;\
[label sizeToFit];

//rightBarButtonItem自定义
#define RIGHTBARBTN(content,method)\
UIBarButtonItem *rightButton = [[UIBarButtonItem alloc] initWithTitle:content style:UIBarButtonItemStyleDone target:self action:@selector(method)];\
[rightButton setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:[UIFont systemFontOfSize:17.0],NSFontAttributeName,RGB_COLOR(112, 182, 35), NSForegroundColorAttributeName, nil] forState:UIControlStateNormal];\
self.navigationItem.rightBarButtonItem = rightButton;

#define isIOS7    ( [[[UIDevice currentDevice] systemVersion] compare:@"7.0"] != NSOrderedAscending )
#endif
```