---
layout: post
#ZXPAutoLayout_autoLayout的三方_国产
title:  ZXPAutoLayout_autoLayout的三方_国产
#时间配置
date:   2016-09-06 19:29:50 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

### 相关资料
---

* <a href="https://github.com/biggercoffee/ZXPAutoLayout" target="_blank">三方git库</a><br>
* <a href="http://www.okbase.net/file/item/33680" target="_blank">自动布局神器--ZXPAuaoLayout框架,一行代码搞定autolayout(iOS源代码)</a><br>
* <a href="http://www.cocoachina.com/ios/20160113/14974.html" target="_blank">最轻巧的自动布局--ZXPAutoLayout框架</a><br>

> 模仿<a href="https://github.com/SnapKit/Masonry" target="_blank">masonry_github</a>这个三方，不过有些语法相对简单些

### 导入三方
---

##### 1> 直接<a href="https://github.com/biggercoffee/ZXPAutoLayout" target="_blank">ZXPAutoLayout_github</a>上下载导入

##### cocopods导入

```
pod search ZXPAutoLayout
```
![752372-20160907100806223-1806636829.png]({{ site.img_url }}15F11C9802BA4B83669BAADC052F9533.png)

![752372-20160907100839113-1467900724.png]({{ site.img_url }}2B3874B7952ED1563FA2DCD95CC5D035.png)

##### 3> 添加头  #import "ZXPAutoLayout.h" 可以使用

`简单示例`

```objc
// 1. 设置一个红色的view，与self.view保持一致，并适配各个iPhone机型和横竖屏
    UIView *bgView = [UIView new];
    [self.view addSubview:bgView];
    bgView.backgroundColor = [[UIColor redColor] colorWithAlphaComponent:.5];
    [bgView zxp_addConstraints:^(ZXPAutoLayoutMaker *layout) {
        
//        layout.edgeEqualTo(self.view);
        
//        layout.edgeInsets(UIEdgeInsetsMake(0, 0, 0, 0));
        
        layout.topSpace(10).leftSpace(10).bottomSpace(10).rightSpace(10);
        
    }];
    
    // 2. 设置一个蓝色view，设置在superview里的距离和设置自身的宽高
    UIView *blueView = [UIView new];
    [bgView addSubview:blueView];
    blueView.backgroundColor = [UIColor blueColor];
    [blueView zxp_addConstraints:^(ZXPAutoLayoutMaker *layout) {
        
        layout.topSpace(20).leftSpace(20).rightSpace(20).heightValue(100);
    }];
    
    // 3. 设置一个灰色view，设置参照于其他view的距离和等宽等距离属性
    UIView *grayView = [UIView new];
    [bgView addSubview:grayView];
    grayView.backgroundColor = [UIColor grayColor];
    [grayView zxp_addConstraints:^(ZXPAutoLayoutMaker *layout) {
        
        layout.topSpaceByView(blueView,10);
        layout.leftSpaceEqualTo(blueView,0);
        layout.widthEqualTo(blueView,0).multiplier(0.5);
        layout.heightValue(40);
    }];
    
    // 3. UILabel的文字自适应，只需要设置autoHeight属性即可
    UILabel *label = [UILabel new];
    [self.view addSubview:label];
    label.backgroundColor = [UIColor purpleColor];
    label.textColor = [UIColor whiteColor];
    label.text = @"这是文字自胜英积分拉得快圣诞节福利卡积分垃圾发打算离开了就发到；了就发； 是大家发垃圾";
    [label zxp_addConstraints:^(ZXPAutoLayoutMaker *layout) {
        // 设置上边距在grayView的下边，并且加10的距离
        layout.xCenterByView(grayView, 10);
        layout.bottomSpace(20);
        layout.widthValue(100);
        layout.autoHeight();
        
    }];
```

#### 特有的一点，一行代码搞定tableViewCell的自适应

> 即：在heghtForRow方法中直接回调： [tableView zxp_cellHeightWithindexPath:indexPath space:10]

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    return [tableView zxp_cellHeightWithindexPath:indexPath space:10];
}
```

> 方法原理：获取cell的所有的subview，然后再比较各个frame中Y+height比较大的，取出作为高度适应返回值

**模拟：**<br>

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    CGFloat height = [self cellHeightOfCell:tableView indexPath:indexPath andBottomSpace:10];
    return height;
}

#pragma mark 获取高度数值
- (CGFloat)cellHeightOfCell:(UITableView *)tableView indexPath:(NSIndexPath *)indexPath andBottomSpace:(CGFloat)space {
    
    NSAssert([tableView.dataSource respondsToSelector:@selector(tableView:cellForRowAtIndexPath:)], @"请实现 tableView:cellForRowAtIndexPath: 方法");
    
    // 获取cell
    UITableViewCell *cell = [tableView.dataSource tableView:tableView cellForRowAtIndexPath:indexPath];
    
    CGRect cellFrame = cell.frame;
    cellFrame.size.width = CGRectGetWidth(tableView.frame);
    cell.frame = cellFrame;
    
    [cell layoutIfNeeded];
    
    // 遍历所有subview，进行比较
    NSMutableArray *maxArray = [NSMutableArray array];
    for (UIView *view in cell.contentView.subviews) {
        
        [maxArray addObject:@(CGRectGetMaxY(view.frame))];
    }
    
    // 排序方法，从小到大的排序
    NSArray *compareMaxYArray = [maxArray sortedArrayUsingSelector:@selector(compare:)];
    // 返回最大值
    return [[compareMaxYArray lastObject] floatValue] + space;
}
```