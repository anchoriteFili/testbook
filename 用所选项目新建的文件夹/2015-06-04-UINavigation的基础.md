---
layout: post
#标题
title:  UINavigation的基础
#时间配置
date:   2015-06-04 19:06:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
/*
     UINavigationController:导航视图控制器，负责管理具有层次结构的视图，是管理控制器的控制器。其他内容分为两部分：一部分是导航条(navigationBar),另一部分是内容(contentView).
     导航视图控制器维持了一个栈结构，当push到另外一个viewController时，会执行入栈操作；当回上一个viewController或返回指定viewController时，会执行出栈操作
     UIVavigationBar:导航条，继承自UIView,在MVC中位于V层，负责标题，按钮的显示。其内部也维持了一个栈结构，当push页面时，navigationBar会执行入栈的操作，navigationBar会将viewController中的navigationItem属性取出，放入栈中，并显示navigationnItem中的相关信息；pop页面时，出栈，将navigationItem从栈中移除，并移除的按钮，标题等。
     UINavigationItem:UIViewControll的属性，主要用于为NavigationBar提供数据，在MVC中，属于M层，类似于一个包裹，内部存放了导航条上显示的按钮，标题等信息。当显示该ViewController时，navigationItem内部信息会被navigationBar显示。
     UIBarButtonItem:封装了NavigationBar上的按钮信息。在MVC中属于M层（不继承UIView）。NavigationBar在拿到navigationItem后，会将里面的BarButtonItem取出，根据BarButtonItem创建UIButton对象，显示在导航条上
     
     
     */
    
    
    self.navigationItem.title = @"首页";
    
    UIView *titleView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 100, 30)];
    titleView.backgroundColor = [UIColor redColor];
    
    self.navigationItem.titleView = titleView;
    
//    设置左按钮
    UIBarButtonItem *leftBtn = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemCompose target:self action:@selector(leftItemClicked:)];
    self.navigationItem.leftBarButtonItem = leftBtn;
    [leftBtn release];
    
    UIImage *image = [[UIImage imageNamed:@"camera.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    
    UIBarButtonItem *rightBtn = [[UIBarButtonItem alloc] initWithImage:image style:UIBarButtonItemStylePlain target:self action:@selector(rightItemClicked:)];
    
    self.navigationItem.rightBarButtonItem = rightBtn;
    [rightBtn release];
    
//    创建三个UIBarButtonItem
    UIBarButtonItem *item1 = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemPlay target:self action:nil];
    
    UIBarButtonItem *item2 = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemPlay target:self action:nil];
    
    UIBarButtonItem *item3 = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemPlay target:self action:nil];
    
    NSArray *items = [NSArray arrayWithObjects:item1,item2,item3, nil];
    
    self.navigationItem.leftBarButtonItems = items;
    
    [item1 release];
    [item2 release];
    [item3 release];
```

**快速自定义navigation左右中间的元素**

<a href="http://blog.csdn.net/zhuzhihai1988/article/details/7705308" target="_blank">关于NavigationBar背景图片和颜色的设置</a><br>


**创建自定义的宏**

```objc
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
label.textColor = [UIColor orangeColor];\
self.navigationItem.titleView = label;\
label.text = title;\
[label sizeToFit];

//rightBarButtonItem自定义
#define RIGHTBARBTN(content,method)\
UIBarButtonItem *rightButton = [[UIBarButtonItem alloc] initWithTitle:content style:UIBarButtonItemStyleDone target:self action:@selector(method)];\
[rightButton setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:[UIFont fontWithName:@"Helvetica-Bold" size:17.0],NSFontAttributeName,[UIColor greenColor], NSForegroundColorAttributeName, nil] forState:UIControlStateNormal];\
self.navigationItem.rightBarButtonItem = rightButton;
```

**页面快速创建**

```objc
#pragma mark 创建左边的按钮
    UIBarButtonItem *leftButton = [[UIBarButtonItem alloc] initWithTitle:@"<" style:UIBarButtonItemStylePlain target:self action:@selector(clickLeftButton:)];
   
    [leftButton setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:
                                        [UIFont fontWithName:@"Helvetica-Bold" size:17.0], NSFontAttributeName,
                                        [UIColor greenColor], NSForegroundColorAttributeName,
                                        nil]
                              forState:UIControlStateNormal];
    self.navigationItem.leftBarButtonItem = leftButton;

#pragma mark 左边的点击事件
- (void)clickLeftButton:sender {
    
    [self.navigationController popToRootViewControllerAnimated:YES];
    
}
    
#pragma mark 创建右边的按钮
    UIBarButtonItem *rightButton = [[UIBarButtonItem alloc] initWithTitle:@"评论" style:UIBarButtonItemStyleDone target:self action:@selector(clickRightButton:)];
    [rightButton setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:
                                         [UIFont fontWithName:@"Helvetica-Bold" size:17.0], NSFontAttributeName,
                                         [UIColor greenColor], NSForegroundColorAttributeName,
                                         nil]
                               forState:UIControlStateNormal];
    self.navigationItem.rightBarButtonItem = rightButton;

#pragma mark 右边的点击事件
- (void)clickRightButton:sender {
    RightViewController *rightVC = [[RightViewController alloc] init];
    [self.navigationController pushViewController:rightVC animated:YES];
}

#pragma mark 图片设置
    UIImage *image = [[UIImage imageNamed:@"home_tabbar_icon_home"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    UIBarButtonItem *leftButton = [[UIBarButtonItem alloc ] initWithImage:image style:UIBarButtonItemStylePlain target:self action:@selector(leftBtnClick:)];
    self.navigationItem.leftBarButtonItem = leftButton;

#pragma mark 左边的点击事件
- (void)leftBtnClick:sender {
    [self.navigationController popViewControllerAnimated:YES];
}
```