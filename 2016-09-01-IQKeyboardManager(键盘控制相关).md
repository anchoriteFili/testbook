---
layout: post
#IQKeyboardManager(键盘控制相关)
title:  IQKeyboardManager(键盘控制相关)
#时间配置
date:   2016-09-01 14:03:50 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

#### 相关引用
---

* <a href="https://github.com/hackiftekhar/IQKeyboardManager" target="_blank">IQKeyboardManager</a><br>
* <a href="http://my.oschina.net/u/1418722/blog/384477" target="_blank">自动处理键盘事件的第三方库 IQKeyboardManager</a><br>
* <a href="http://files.cnblogs.com/files/AnchoriteFiliGod/IQKeyboardManager使用.zip" target="_blank">IQKeyboardManager使用.zip</a><br>
* <a href="http://www.jianshu.com/p/9d7d246bd350/comments/1518291" target="_blank">浅谈IQKeyboardManager第三方库的使用</a><br>

#### 使用方法
---

##### 1. 创建工程导入第三方

```
pod 'IQKeyboardManager'
```

> 导入第三方以后默认的是所有的页面就已经有所有效果了，这个就尴尬了，必须在不用的页面去关闭键盘的控制

##### 2. 设置相关参数，键盘的各个参数进行控制

```objc
IQKeyboardManager *manager = [IQKeyboardManager sharedManager];
manager.enable = YES; // 控制整个功能是否开启
manager.shouldResignOnTouchOutside = YES; // 控制点击背景是否收起键盘
manager.shouldToolbarUsesTextFieldTintColor = NO; // 控制键盘上的工具条文字亚瑟是否用户自定义
manager.enableAutoToolbar = NO; // 控制是否显示键盘上的工具条
```