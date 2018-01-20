---
layout: post
#标题
title:  weboss首页自定义tabBar模拟
#时间配置
date:   2015-10-21 15:51:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/自定义TabBar并实现跳转.zip" target="_blank">自定义TabBar并实现跳转.zip</a><br>



**结构分析：**

##### 1. 创建一个UITabBarController，直接在storyboard中添加其为首页，在其中添加tabBarController的各个子页面

##### 2. 创建一个继承自UIVIewController的模板页面，在模板页面中写出其所有子页面所共有的元素，即自定义的tabBarView

```objc
UIButton *redButton = [UIButton buttonWithType:UIButtonTypeCustom];
    redButton.frame = CGRectMake(0, self.view.frame.size.height-40, self.view.frame.size.width/3, 40);
    redButton.backgroundColor = [UIColor blueColor];
    [redButton addTarget:self action:@selector(redButtonClick:) forControlEvents:UIControlEventTouchUpInside];
    [redButton setTitle:@"红页面" forState:UIControlStateNormal];
    [self.view addSubview:redButton];
    
    UIButton *blueButton = [UIButton buttonWithType:UIButtonTypeCustom];
    blueButton.frame = CGRectMake(self.view.frame.size.width/3, self.view.frame.size.height-40, self.view.frame.size.width/3, 40);
    blueButton.backgroundColor = [UIColor orangeColor];
    [blueButton addTarget:self action:@selector(blueButtonClick:) forControlEvents:UIControlEventTouchUpInside];
    [blueButton setTitle:@"蓝页面" forState:UIControlStateNormal];
    [self.view addSubview:blueButton];
    
    UIButton *yellowButton = [UIButton buttonWithType:UIButtonTypeCustom];
    yellowButton.frame = CGRectMake(self.view.frame.size.width/3*2, self.view.frame.size.height-40, self.view.frame.size.width/3, 40);
    yellowButton.backgroundColor = [UIColor redColor];
    [yellowButton addTarget:self action:@selector(yellowButtonClick:) forControlEvents:UIControlEventTouchUpInside];
    [yellowButton setTitle:@"黄页面" forState:UIControlStateNormal];
    [self.view addSubview:yellowButton];
```

##### 3. 根据母版创建出TabBar的所有的子页面，让其共有同样的底部tabBar

##### 4. 将所有的子页面都加入到UITabBarController中


```objc
/**
     将自己赋值给appDelegate中的mainVC，从而能够让其他页面可以通过appDelegate中的方法jumpVC来控制本页面的jumpVCHEHE方法，从而实现跳转
     */
    AppDelegate *appDelegate = (AppDelegate*)[[UIApplication sharedApplication] delegate];
    appDelegate.mainVC = self;
    
    RedViewController *redVC = [[RedViewController alloc] init];
    UINavigationController *redNavigationController = [[UINavigationController alloc] initWithRootViewController:redVC];
    redVC.title = @"红页面";
    
    BlueViewController *blueVC = [[BlueViewController alloc] init];
    UINavigationController *blueNavigationController = [[UINavigationController alloc] initWithRootViewController:blueVC];
    blueVC.title = @"蓝页面";
    
    YellowViewController *yellowVC = [[YellowViewController alloc] init];
    UINavigationController *yellowNavigationController = [[UINavigationController alloc] initWithRootViewController:yellowVC];
    yellowVC.title = @"黄页面";
    
#pragma mark 将所有的页面添加到viewControllers中，让主页面tabBar进行显示
    self.viewControllers = [NSArray arrayWithObjects:
                            redNavigationController,
                            blueNavigationController,
                            yellowNavigationController,
                            nil];
    /**
     这是tabBar的变异体
     */
    self.selectedIndex = 2;
```

##### 5. 下面的模拟tabBar传值顺序：母版页面点击相应位置的button，调用appDelegate方法中的方法，从而控制TabBarController中的方法，实现跳转

###### 1》首先将模板赋值给appDelegate

```objc
 /**
      将自己赋值给appDelegate中的mainVC，从而能够让其他页面可以通过appDelegate中的方法jumpVC来控制本页面的jumpVCHEHE方法，从而实现跳转
      */
     AppDelegate *appDelegate = (AppDelegate*)[[UIApplication sharedApplication] delegate];
     appDelegate.mainVC = self;
```

###### 2》触发点击事件，调用appDelegate中的方法

```objc
#pragma mark 红按钮点击事件
- (void)redButtonClick:(UIButton *)sender {
    AppDelegate *appDelegate = (AppDelegate*)[[UIApplication sharedApplication] delegate];
    [appDelegate jumpVC:0];
}

//appDelegate中触发方法
#pragma mark 中转作用的方法
- (void)jumpVC:(NSUInteger)index {
    if (self.mainVC) {
        [self.mainVC jumpVCHEHE:index];
    }
} 

//tabBarController中触发方法，实现跳转
#pragma mark 其他页面传过来的控制方法，用于页面的选择跳转
- (void)jumpVCHEHE:(NSUInteger)index {
    self.selectedIndex = index;
}
```

**注意：引用#import "AppDelegate.h"时一定要写在.m文件中，否则会出现找不到母版的报错，很坑**