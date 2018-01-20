---
layout: post
#masonry__轻量级AutoLayout三方框架
title:  masonry__轻量级AutoLayout三方框架
#时间配置
date:   2016-09-05 18:06:50 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

### 参考资料
---

* <a href="https://github.com/SnapKit/Masonry" target="_blank">git地址(文档内容最具参考价值)</a><br>
* <a href="http://www.huangyibiao.com/archives/153" target="_blank">Masonry基本用法</a><br>
* <a href="http://blog.csdn.net/yxwlzsh/article/details/46549513" target="_blank">Masonry 和 CocoaPods 介绍及安装步骤</a><br>
* <a href="http://blog.csdn.net/woaifen3344/article/details/50114373" target="_blank">Masonry自动布局详解一：基本用法</a><br>
* <a href="http://www.cocoachina.com/ios/20141219/10702.html" target="_blank">Masonry介绍与使用实践：快速上手Autolayout</a><br>
* <a href="http://www.jianshu.com/p/d172131d24c9" target="_blank">Masonry使用心得</a><br>
* <a href="Masonry_swift版_SnapKit" target="_blank">Masonry_swift版_SnapKit</a><br>
* <a href="http://files.cnblogs.com/files/AnchoriteFiliGod/masonry测试.zip" target="_blank">masonry测试.zip</a><br>

#### 1. 导入框架
---

##### 1> 直接到到masonry_github上直接下载并导入
##### 2> 使用cocopods进行下载
> 终端 - > $ pod search masonry -> 加入Podfile -> 下载

![752372-20160907100936113-781576705.png]({{ site.img_url }}A5639E58EB0AC02C619236FD69782EE5.png)

![752372-20160907101003848-1556676271.png]({{ site.img_url }}5A361955DE9AB1286351714F9D448B7B.png)

#### 2. 将  #import "masonry.h" 头导入项目即可使用

![752372-20160907101044660-1279938758.png]({{ site.img_url }}521DF4DBA5B343255CA4D085312DC45E.png)

> 所有控件添加约束前，需对控件`translatesAutoresizingMaskIntoConstraints`属性赋值为`NO`,且先`先添加到父类再添加约束`。

```objc
self.view1.translatesAutoresizingMaskIntoConstraints = NO; // 要先赋值为NO
```

#### 3. 官方文档相关语法介绍

> **语法1：** make.top.equalTo(superview.mas_top).with.offset(padding.top)

`用于设置相对某个控件的偏移量，可以用于上下左右相对位置，例：`

```objc
UIEdgeInsets padding = UIEdgeInsetsMake(10, 10, 10, 10);

[view1 mas_makeConstraints:^(MASConstraintMaker *make) {
    make.top.equalTo(superview.mas_top).with.offset(padding.top); //with is an optional semantic filler
    make.left.equalTo(superview.mas_left).with.offset(padding.left);
    make.bottom.equalTo(superview.mas_bottom).with.offset(-padding.bottom);
    make.right.equalTo(superview.mas_right).with.offset(-padding.right);
}];
```

> **语法2：** make.edges.equalTo(superview).with.insets(padding)

`效果同语法1相同，可以同时设置上下左右相对位置，例：`

```objc
[view1 mas_makeConstraints:^(MASConstraintMaker *make) {
    make.edges.equalTo(superview).with.insets(padding);
}];
```

> **语法3：** greaterThanOrEqualTo(label)

`设置某个元素大于或等于label这个控件，例：`

```objc
//下面的两个方法有相同的效果
[view1 mas_makeConstraints:^(MASConstraintMaker *make) {
　　make.left.greaterThanOrEqualTo(label);
　　make.left.greaterThanOrEqualTo(label.mas_left);
}];
```

> **语法4：** lessThanOrEqualTo(@400)

`同语法3，直接添加数字，没有参考控件，此语法不能直接添加数字，要加"@"，例：`

```objc
//设置width >= 200 && width <= 400
make.width.greaterThanOrEqualTo(@200);
make.width.lessThanOrEqualTo(@400)
```

> **语法5：** mas_equalTo(42)

`区别于语法4，此语法可以直接在括号中设置固定数值进行固定位置，例：`

```objc
make.top.mas_equalTo(42);
make.height.mas_equalTo(20);
make.size.mas_equalTo(CGSizeMake(50, 100));
make.edges.mas_equalTo(UIEdgeInsetsMake(10, 0, 10, 0));
make.left.mas_equalTo(view).mas_offset(UIEdgeInsetsMake(10, 0, 10, 0));
```

> **语法6：** priority

`优先级属性，貌似平常没用过，想了解请参考` <a href="http://book.51cto.com/art/201502/464885.htm" target="_blank">枚举型优先级</a>

```objc
.priority 允许您指定一个精确的优先级
.priorityHigh 相当于 UILayoutPriorityDefaultHigh = 750
.priorityMedium 优先级居于 750 和 250之间
.priorityLow 相当于 UILayoutPriorityDefaultLow = 250
```

```objc
make.left.greaterThanOrEqualTo(label.mas_left).with.priorityLow();
make.top.equalTo(label.mas_top).with.priority(600);
```

> **语法7：** edges.equalTo(view2)

`edges的两个相关语法`

```objc
// 直接和view2完全重合
make.edges.equalTo(view2);

// 直接设置相对父控件的edges
make.edges.equalTo(superview).insets(UIEdgeInsetsMake(5, 10, 15, 20))
```

> **语法8：** size.greaterThanOrEqualTo(titleLabel)

`size 的相关语法`

```objc
// 设置宽高大于等于titleLabel，此为动态语法，表示size可以重新赋值
make.size.greaterThanOrEqualTo(titleLabel)

// 直接设置相对父控件的宽高值
make.size.equalTo(superview).sizeOffset(CGSizeMake(100, -50))
```

> **语法9：** center.equalTo(button1)

`center相关语法`

```objc
// 中心center与button1相同
make.center.equalTo(button1)

// 直接设置相对父控件的中心偏移
make.center.equalTo(superview).centerOffset(CGPointMake(-5, 10))
```

> **语法10：** .

`直接连 . 语法`

```objc
// 直接设置左右底部与父控件相等
make.left.right.and.bottom.equalTo(superview);
make.top.equalTo(otherView);
```

> **语法11：** MASConstraint约束属性相关

`对MASConstraint变量赋值，即可直接使用，例：`

```objc
// 创建头部约束变量
@property (nonatomic, strong) MASConstraint *topConstraint;

// 对头部约束变量赋值
[view1 mas_makeConstraints:^(MASConstraintMaker *make) {
    self.topConstraint = make.top.equalTo(superview.mas_top).with.offset(padding.top);
    make.left.equalTo(superview.mas_left).with.offset(padding.left);
}];

// 可直接改变头部便宜量，从而控制控件的居上距离
[UIView animateWithDuration:2 animations:^{
        self.topConstraint.offset = 100; // 从新设置view1的居上的偏移量，使其等于100
        [self.view layoutIfNeeded]; // 只有添加了这个方法，才能有动画
}];
```

> **语法12：** mas_updateConstraints

> 用于更新约束的方法，可对某控件中各个约束条件进行更新，需要注意的是，此方法不会清除老的约束，
>
> 即，很有可能造成新老约束冲突报错造成失效，因此使用此方法时，老的约束条件应该是范围的约束，
>
> 不推荐使用此方法，推荐使用mas_remakeConstraints方法，例：

```objc
// 屏宽、高30、居父类左0上50
[self.view1 mas_makeConstraints:^(MASConstraintMaker *make) {
      make.width.equalTo(self.view); 
      make.height.mas_greaterThanOrEqualTo(@30); // 此时可以设置大于等于30的高度
      make.left.equalTo(self.view1.superview.mas_left).with.offset(0); // 设置居父类左边偏移量为0
        // view1居上距离属性赋值
      self.topConstraint = make.top.equalTo(self.view1.superview.mas_top).with.offset(50);
        
}];

[UIView animateWithDuration:2 animations:^{
      // 对控件重新布局
      [self.view1 mas_updateConstraints:^(MASConstraintMaker *make) {
          make.top.mas_offset(100);
          make.height.mas_equalTo(300);
          make.width.mas_equalTo(300);
          make.left.mas_equalTo(10);
      }];
      [self.view layoutIfNeeded];
 }];
```

> **语法13：** mas_remakeConstraints

> mas_updateConstraints有其缺点，就是进行新的约束更新的时候，老的约束还是存在的，因此可能会造成约束冗余，
>
> 而mas_remakeConstraints在执行之前，会把老的约束全部清除掉再从新添加新的约束，即新老约束没有联系，例：

```objc
// 屏宽、高30、居父类左0上50
[self.view1 mas_makeConstraints:^(MASConstraintMaker *make) {
      make.width.equalTo(self.view); 
      make.height.mas_greaterThanOrEqualTo(@30); // 此时可以设置大于等于30的高度
      make.left.equalTo(self.view1.superview.mas_left).with.offset(0); // 设置居父类左边偏移量为0
        // view1居上距离属性赋值
      self.topConstraint = make.top.equalTo(self.view1.superview.mas_top).with.offset(50);
        
}];

[UIView animateWithDuration:2 animations:^{
      // 对控件重新布局
      [self.view1 mas_remakeConstraints:^(MASConstraintMaker *make) {
          make.top.mas_offset(100);
          make.height.mas_equalTo(300);
          make.width.mas_equalTo(300);
          make.left.mas_equalTo(10);
      }];
      [self.view layoutIfNeeded];
 }];
```

> **语法14：** make.height.equalTo(self.view1.mas_width).multipliedBy(0.3);

`比例语法，高/宽比例为0.3，例：`

```objc
    // 与view1的中心偏移100，宽相等，高为宽的0.3倍
    [view2 mas_makeConstraints:^(MASConstraintMaker *make) {
        
        make.center.equalTo(self.view1).centerOffset(CGPointMake(100, 100));
        make.width.equalTo(self.view1);
        make.height.equalTo(self.view1.mas_width).multipliedBy(0.3);
    }];
```

#### 简单小示例

```objc
    // 左右偏移量为0，上偏移量为20，高度固定30
    [self.view1 mas_makeConstraints:^(MASConstraintMaker *make) {

        make.left.right.mas_offset(0);
        make.top.mas_offset(20);
        make.height.mas_equalTo(30);
    }];
```

```objc
    // 居左上偏移量10，宽高相等为50
    [self.view1 mas_makeConstraints:^(MASConstraintMaker *make) {

        make.left.top.mas_offset(10);
        make.width.height.mas_equalTo(50);
    }];
```

```objc
    // 与view1左对齐，宽高相等，头部距离view1底部10
    [view2 mas_makeConstraints:^(MASConstraintMaker *make) {
        
        make.left.width.height.equalTo(self.view1);
        make.top.equalTo(self.view1.mas_bottom).with.offset(10);
    }];
```

```objc
    // 在view1右侧10，顶部下10，固定宽100，高30
    [view2 mas_makeConstraints:^(MASConstraintMaker *make) {
        
        make.left.equalTo(self.view1.mas_right).mas_offset(10);
        make.top.equalTo(self.view1.mas_top).mas_offset(10);
        make.width.mas_equalTo(100);
        make.height.mas_equalTo(30);
    }];
```

```objc
    // 在view1右侧10，顶部下10，宽高都比view1少10
    [view2 mas_makeConstraints:^(MASConstraintMaker *make) {
        
        make.left.equalTo(self.view1.mas_right).mas_offset(10);
        make.top.equalTo(self.view1.mas_top).mas_offset(10);
        make.size.equalTo(self.view1).sizeOffset(CGSizeMake(-10, -10));
    }];
```

```objc
    // 在view1右侧10，水平方向平行，宽高都比view1少10
    [view2 mas_makeConstraints:^(MASConstraintMaker *make) {
        
        make.left.equalTo(self.view1.mas_right).mas_offset(10);
        make.centerY.equalTo(self.view1);
        make.size.equalTo(self.view1).sizeOffset(CGSizeMake(-10, -10));
    }];
```

```objc
    // 与view1的中心偏移100，宽相等，高为宽的0.3倍
    [view2 mas_makeConstraints:^(MASConstraintMaker *make) {
        
        make.center.equalTo(self.view1).centerOffset(CGPointMake(100, 100));
        make.width.equalTo(self.view1);
        make.height.equalTo(self.view1.mas_width).multipliedBy(0.3);
    }];
```