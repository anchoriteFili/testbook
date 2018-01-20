---
layout: post
#标题
title:  UIPickerView的使用_可以滚动的视图控件
#时间配置
date:   2015-08-03 20:03:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://www.cnblogs.com/wendingding/p/3771047.html" target="_blank">iOS开发UI篇—使用picker View控件完成一个简单的选餐应用</a><br>
<a href="http://ikrboy.iteye.com/blog/2003127" target="_blank">IOS之简单选择器UIPickerView（省份+城市）</a><br>
<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/UIPickerView测试.zip" target="_blank">UIPickerView测试.zip</a><br>


**UIPickerView快速应用矿建**

```objc
#pragma mark 创建颜色宏
#define RGBA_COLOR(R, G, B, A) [UIColor colorWithRed:((R)/255.0f) green:((G)/255.0f) blue:((B)/255.0f) alpha:A]
#define RGB_COLOR(R, G, B) [UIColor colorWithRed:((R)/255.0f) green:((G)/255.0f) blue:((B)/255.0f) alpha:1.0f]

//属性
@property (nonatomic,retain) UIPickerView *leftPickerView;
@property (nonatomic,retain) NSArray *moneyCountArr;

#pragma mark 创建初始化两个pickerView数值的数组
    self.moneyCountArr = @[@"1000",@"2000",@"5000",@"8000",@"10000"];
    
#pragma mark 添加<UIPickerViewDataSource,UIPickerViewDelegate>
#pragma mark 创建UIPicerView
#pragma mark 创建左pickerView
    self.leftPickerView = [[UIPickerView alloc] initWithFrame:CGRectMake(27.5, 154.5, 159.5, 188)];
    
    self.leftPickerView.dataSource = self;
    self.leftPickerView.delegate = self;
    [self.leftPickerView selectRow:2 inComponent:0 animated:NO]; //默认选中行，要放在后边才会实现
    [self.view addSubview:self.leftPickerView];
```

**方法**

```objc
#pragma mark 设置分区数和每个分区的行数
- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView {
    return 1;
}

- (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component {
    return 5;
}

#pragma mark 设置分区的宽和行高
- (CGFloat)pickerView:(UIPickerView *)pickerView widthForComponent:(NSInteger)component {
    return 120;
    
}

- (CGFloat)pickerView:(UIPickerView *)pickerView rowHeightForComponent:(NSInteger)component {
    return 37;
}

#pragma mark 选中事件
- (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component {
    
}

#pragma mark 自定义pickerView中的label
- (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(UIView *)view {
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 10, 300/2, 30)];
    label.textColor = RGB_COLOR(112, 182, 35);
    label.font = [UIFont systemFontOfSize:14.0];
    label.textAlignment = NSTextAlignmentCenter;
    
    label.text = [self.moneyCountArr objectAtIndex:row];
    
    return label;
}
```