---
layout: post
#标题
title:  在window上添加view模拟lunchImage
#时间配置
date:   2016-04-28 15:23:22 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![752372-20160428151848642-1876527474.png]({{ site.img_url }}193CBA37D55A220D7EFBC61D87FE8D61.png)


<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/登录视图的创建.zip" target="_blank">登录视图的创建.zip</a><br>

**代码部分**

```objc
#pragma mark 各种尺寸的图片
#define foureSize ([UIScreen mainScreen].bounds.size.height == 480)
#define fiveSize ([UIScreen mainScreen].bounds.size.height == 568)
#define sixSize ([UIScreen mainScreen].bounds.size.height == 667)
#define sixPSize ([UIScreen mainScreen].bounds.size.height > 668)

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    [self.window makeKeyAndVisible];
    
#pragma mark 这部分一定要写在下面
    self.window.rootViewController = [[BlueViewController alloc] init];
    [self setStartAnamation]; //添加登录图片
    
    return YES;
}

#pragma mark 添加页面显示的动画图片
-(void)setStartAnamation {
    
    UIImageView *luanchImage = [[UIImageView alloc]initWithFrame:[[UIScreen mainScreen] bounds]];
    UIImageView *longLine = [[UIImageView alloc]init];
    UIImageView *yuandian = [[UIImageView alloc]initWithFrame:CGRectMake(0, -4, 10, 10)];
    
    if (foureSize) {
        longLine.frame = CGRectMake([UIScreen mainScreen].bounds.size.width/8, [UIScreen mainScreen].bounds.size.height/2+15, [UIScreen mainScreen].bounds.size.width*3/4, 2);
        luanchImage.image = [UIImage imageNamed:@"4start"];
        longLine.image = [UIImage imageNamed:@"changxian4_5"];
        yuandian.image = [UIImage imageNamed:@"yuandian4_5"];
        
    }else{
        
        longLine.frame = CGRectMake([UIScreen mainScreen].bounds.size.width/8, [UIScreen mainScreen].bounds.size.height/2, [UIScreen mainScreen].bounds.size.width*3/4, 2);
        if(fiveSize){
            luanchImage.image = [UIImage imageNamed:@"5start"];
            longLine.image = [UIImage imageNamed:@"changxian4_5"];
            yuandian.image = [UIImage imageNamed:@"yuandian4_5"];
            
        }else if (sixSize){
            luanchImage.image = [UIImage imageNamed:@"6start"];
            longLine.image = [UIImage imageNamed:@"changxian_6"];
            yuandian.image = [UIImage imageNamed:@"yuandian_6"];
            
        }else if(sixPSize){
            luanchImage.image = [UIImage imageNamed:@"6pstart"];
            longLine.image = [UIImage imageNamed:@"changxian_6p"];
            yuandian.image = [UIImage imageNamed:@"yuandian_6p"];
        }
    }
    
    luanchImage.userInteractionEnabled = YES;
    [self.window addSubview:luanchImage];
    
    [luanchImage addSubview:longLine];
    [longLine addSubview:yuandian];
    
    //加载动画
    [UIView animateWithDuration:2.0 animations:^{
        yuandian.frame = CGRectMake([UIScreen mainScreen].bounds.size.width*3/4, -4, 10, 10);
    } completion:^(BOOL finished) {
        [UIView animateWithDuration:0.5 animations:^{
            luanchImage.alpha = 0;
        } completion:^(BOOL finished) {
            [luanchImage removeFromSuperview];
        }];
    }];
}
```