---
layout: post
#标题
title: UIAppearance使用详解
#时间配置
date:   2015-12-09 13:51:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

#### 相关链接
---

* <a href="http://blog.sina.com.cn/s/blog_9693f61a0101f1rs.html" target="_blank">iOS UIAppearance使用详解</a><br>
* <a href="http://files.cnblogs.com/files/AnchoriteFiliGod/UIAppearance使用详解.zip" target="_blank">UIAppearance使用详解.zip</a><br>

> iOS5及其以后提供了一个比较强大的工具UIAppearance，我们通过UIAppearance设置一些UI的全局效果，这样就可以很方便的实现UI的自定义效果，又能最简单的饿实现统一的界面风格。

**方法详解：**

```objc
+ (id)appearance
```

> 这个方法是统一全部改，比如你设置UINavBar的tintColor，你可以这样写：[[UINavigationBar appearance] setTintColor:myColor];

> 注意：使用appearance设置UI效果最好采用全局的设置，在所有界面初始化前开始设置，否则可能无效，例如：可以写在appdelegate中的“didFinishLaunchingWithOptions”方法中

```objc
 + (id)appearanceWhenContainedIn:(Class<>)ContainerClass,...
 ```

> 这个方法可以设置某个类的改变：例如：设置UIBarButtonItem在UINavigationBar、UIPopoverController、UITabbar 中的效果。就可以这样写：

```objc
[[UIBarButtonItem appearanceWhenContainedIn:[UINavigationBar class], [UIPopoverController class],[UITabbar class] nil] setTintColor:myPopoverNavBarColor];
```

![752372-20151209174438949-230522542.png]({{ site.img_url }}7F03A18F4041E70F21E401269AD68590.png)


```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
#pragma mark ======修改导航栏背景及字体======
#pragma mark 设置导航条的背景图
    [[UINavigationBar appearance] setBackgroundImage:[UIImage imageNamed:@"导航条"] forBarMetrics:UIBarMetricsDefault];
#pragma mark 设置导航条的颜色
    [[UINavigationBar appearance] setBackgroundColor:[UIColor greenColor]];
#pragma mark 设置导航条的字体
    /**
     //    Keys for Text Attributes Dictionaries
     //    NSString *const UITextAttributeFont;                  value: UIFont
     //    NSString *const UITextAttributeTextColor;             value: UIColor
     //    NSString *const UITextAttributeTextShadowColor;       value: UIColor
     //    NSString *const UITextAttributeTextShadowOffset;      value: NSValue wrapping a UIOffset struct.
     */
    NSDictionary *dic = [NSDictionary dictionaryWithObjectsAndKeys:[UIColor yellowColor],NSForegroundColorAttributeName,[UIFont fontWithName:@ "HelveticaNeue-CondensedBlack" size:18.0],NSFontAttributeName, nil];
    [[UINavigationBar appearance] setTitleTextAttributes:dic];
    
#pragma mark ======标签栏（UITabbar）======
#pragma mark tabBar的背景图片
    [[UITabBar appearance] setBackgroundImage:[UIImage imageNamed:@"导航条"]];
#pragma mark 设置选择item的背景图片 图片跟着点击事件进行移动
    UIImage *selectBackgroundImage = [[UIImage imageNamed:@"tabBar图标"] resizableImageWithCapInsets:UIEdgeInsetsMake(4, 0, 0, 0)];
    [[UITabBar appearance] setSelectionIndicatorImage:selectBackgroundImage];

#pragma mark ======分段控件（UISegmentControl）======
    UISegmentedControl *segmentControlAppearance = [UISegmentedControl appearance];
    
    //segment正常背景
    [segmentControlAppearance setBackgroundImage:[UIImage imageNamed:@"segmentImage"] forState:UIControlStateNormal barMetrics:UIBarMetricsDefault];
    
    //segment选中背景
    [segmentControlAppearance setBackgroundImage:[UIImage imageNamed:@"segmentSelectImage"] forState:UIControlStateSelected barMetrics:UIBarMetricsDefault];
    
    //segment左右选中/没选中时的分割线
    /**
     //BarMetrics表示navigation bar的状态，UIBarMetricsDefault表示portrait状态(44pixel height)，UIBarMetricsLandscapePhone
     //表示landscape状态(32pixel heght）
     */
    
    [segmentControlAppearance setDividerImage:[UIImage imageNamed:@"line"] forLeftSegmentState:UIControlStateNormal rightSegmentState:UIControlStateNormal barMetrics:UIBarMetricsDefault];
    [segmentControlAppearance setDividerImage:[UIImage imageNamed:@"line"] forLeftSegmentState:UIControlStateSelected rightSegmentState:UIControlStateNormal barMetrics:UIBarMetricsDefault];
    [segmentControlAppearance setDividerImage:[UIImage imageNamed:@"line"] forLeftSegmentState:UIControlStateNormal rightSegmentState:UIControlStateSelected barMetrics:UIBarMetricsDefault];
    
#pragma mark 点击状态的时候的字体的设置
    NSDictionary *textAttributes1 = @{UITextAttributeFont: [UIFont systemFontOfSize:18],
                                      UITextAttributeTextColor: [UIColor blueColor],
                                      UITextAttributeTextShadowColor: [UIColor whiteColor],
                                      UITextAttributeTextShadowOffset: [NSValue valueWithCGSize:CGSizeMake(1, 1)]};
    
    [segmentControlAppearance setTitleTextAttributes:textAttributes1 forState:1];
    
#pragma mark 正常状态时字体的设置
    NSDictionary *textAttributes2 = @{UITextAttributeFont: [UIFont systemFontOfSize:18],
                                      UITextAttributeTextColor: [UIColor whiteColor],
                                      UITextAttributeTextShadowColor: [UIColor blackColor],
                                      UITextAttributeTextShadowOffset: [NSValue valueWithCGSize:CGSizeMake(1, 1)]};
    
    [segmentControlAppearance setTitleTextAttributes:textAttributes2 forState:0];
    
#pragma mark ======UIBarbutton======
    /**
     UIBarbutton有leftBarButton，rightBarButton和backBarButton，其中backBarButton由于带有箭头，需要单独设置
     */
    
#pragma mark 修改导航条上的UIBarButtonItem
    UIBarButtonItem *buttonItemAppearance = [UIBarButtonItem appearanceWhenContainedInInstancesOfClasses:@[[UINavigationBar class]]];
    
    //设置导航栏的字体包括backBarButton和leftBarButton,rightBarButton的字体
    NSDictionary *textAttributesOne = @{UITextAttributeFont: [UIFont systemFontOfSize:20],
                                     UITextAttributeTextColor: [UIColor orangeColor],
                                     UITextAttributeTextShadowColor: [UIColor whiteColor],
                                     UITextAttributeTextShadowOffset: [NSValue valueWithCGSize:CGSizeMake(1, 1)]};
    [buttonItemAppearance setTitleTextAttributes:textAttributesOne forState:0]; //forState为0时为正常状态，为1时为点击状态
    
    NSDictionary *textAttributesTwo = @{UITextAttributeFont: [UIFont systemFontOfSize:20],
                                     UITextAttributeTextColor: [UIColor yellowColor],
                                     UITextAttributeTextShadowColor: [UIColor whiteColor],
                                     UITextAttributeTextShadowOffset: [NSValue valueWithCGSize:CGSizeMake(1, 1)]};
    [buttonItemAppearance setTitleTextAttributes:textAttributesTwo forState:1]; //forState为0时为正常状态，为1时为点击状态
    
#pragma mark 修改leftBarButton,rightBarButton背景效果
    [buttonItemAppearance setBackgroundImage:[UIImage imageNamed:@"右边按钮"]
                                    forState:UIControlStateNormal
                                       style:UIBarButtonItemStylePlain
                                  barMetrics:UIBarMetricsDefault];
    
    [buttonItemAppearance setBackgroundImage:[UIImage imageNamed:@"右边按钮"]
                                    forState:UIControlStateHighlighted
                                       style:UIBarButtonItemStylePlain
                                  barMetrics:UIBarMetricsDefault];

#pragma mark backBarButton需要单独设置背景效果。
    [buttonItemAppearance setBackButtonBackgroundImage:[UIImage imageNamed:@"返回按钮"]
                                              forState:0
                                            barMetrics:UIBarMetricsDefault];
    [buttonItemAppearance setBackButtonBackgroundImage:[UIImage imageNamed:@"返回按钮"]
                                              forState:1
                                            barMetrics:UIBarMetricsDefault];
    
    [buttonItemAppearance setBackButtonTitlePositionAdjustment:UIOffsetMake(2, -1)
                                                 forBarMetrics:UIBarMetricsDefault];
    
#pragma mark ======工具栏（UIToolbar）======
    UIToolbar *toolbarAppearance = [UIToolbar appearance];
    
    /**
     样式和背景二选一即可，看需求
     样式(黑色半透明，不透明等)设置
     */
//    [toolbarAppearance setBarStyle:UIBarStyleBlackTranslucent];
    //背景设置
    [toolbarAppearance setBackgroundImage:[UIImage imageNamed:@"导航条"]
                       forToolbarPosition:UIToolbarPositionAny
                               barMetrics:UIBarMetricsDefault];
    
    return YES;
}
```