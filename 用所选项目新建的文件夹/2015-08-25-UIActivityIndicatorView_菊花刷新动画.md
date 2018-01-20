---
layout: post
#标题
title:  UIActivityIndicatorView -- 菊花刷新动画
#时间配置
date:   2015-08-25 11:42:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/菊花测试.zip" target="_blank">菊花测试.zip</a><br>
<a href="http://blog.csdn.net/ch_soft/article/details/6948306" target="_blank">UIActivityIndicatorView的两种形式</a><br>
<a href="http://www.baidu.com/link?url=R3DMb7KKCZdfV-5z8GlL-iPlku9GzJFyDBdoQ5FUMuHa61pLDS1KyXML2B6gsd34dKgBRb3nA8VqXd48OjlZ6X_Psb2dzHwt7dJoO0EN0Wi&wd=&eqid=8774909100006bf30000000355dbe377" target="_blank">Loading效果 UIActivityIndicatorView - wsjisji - 博客园</a><br>


**快速创建**

```objc
@property (nonatomic,strong) UIActivityIndicatorView *activityIndicatorView; //旋转菊花view
@property (nonatomic,strong) UIView *forwardView; //刷新是蒙版view

#pragma mark 添加动画效果 -- 点击事件
- (IBAction)click:(UIButton *)sender {
    
    [self.view addSubview:self.forwardView];
    self.activityIndicatorView.center = CGPointMake(self.view.bounds.size.width/2, self.view.bounds.size.height/2); //UIActivityIndicatorView大小不能改变，只能设置其center
    [self.activityIndicatorView startAnimating]; //开始动画
    [self.view addSubview:self.activityIndicatorView];
    //1.5秒后执行下面方法
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        [self.activityIndicatorView stopAnimating]; //结束动画
        [self.activityIndicatorView removeFromSuperview]; //移除圆圈view
        [self.forwardView removeFromSuperview]; //移除蒙版view
    });
}

#pragma mark 创建蒙版view
- (UIView *)forwardView {
    if (!_forwardView) {
        _forwardView = [[UIView alloc] init];
        _forwardView.frame = [[UIScreen mainScreen] bounds];
        _forwardView.alpha = 0.5;
        _forwardView.backgroundColor = [UIColor grayColor];
    }
    return _forwardView;
}

#pragma mark 创建圆圈view
- (UIActivityIndicatorView *)activityIndicatorView {
    if (!_activityIndicatorView) {
        _activityIndicatorView = [[UIActivityIndicatorView alloc] initWithActivityIndicatorStyle:UIActivityIndicatorViewStyleWhiteLarge];
        _activityIndicatorView.hidesWhenStopped = YES;
    }
    return _activityIndicatorView;
}
```