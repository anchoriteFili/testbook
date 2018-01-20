---
layout: post
#标题
title:  UITableView整理
#时间配置
date:   2015-06-05 14:35:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

##### 快速创建

```objc
@property (nonatomic,strong) UITableView *tabelView; //全局tableView

#pragma mark 创建tableView <UITableViewDataSource,UITableViewDelegate>
self.tabelView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height) style:UITableViewStylePlain];
    
self.tabelView.delegate = self;
self.tabelView.dataSource = self;
    //    设置tableView背景色为透明色
self.tabelView.backgroundColor = [UIColor clearColor];
    //    分割线样式
    //     self.tabelView.separatorStyle = UITableViewCellSeparatorStyleNone;
//去掉空白cell
self.tabelView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
[self.view addSubview:self.tabelView];

#pragma mark 设置每行的高度
 - (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
 return 60;
 }
 
 #pragma mark 设置section的个数
 - (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
 return 1;
 }
 
 #pragma mark 设置每个section的行数
 - (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
 {
 return 0;
 }
 
 #pragma mark cellForRow方法
 - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
 {
 static NSString *cellIdentifier = @"cell";
 UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
 if (cell == nil) {
 cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
 }
 //    设置选择时的状态
 cell.selectionStyle = UITableViewCellSelectionStyleDefault;
 //    设置cell背景色为透明色
 cell.backgroundColor = [UIColor clearColor];
 
 return cell;
 }

#pragma mark 添加点击cell的方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
    //点击以后直接恢复正常状态
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}
```

**直接获取高度方法**

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    CGFloat height = [self cellHeightOfCell:tableView indexPath:indexPath andBottomSpace:10];
    return height;
}

#pragma mark 获取cell高度数值
- (CGFloat)cellHeightOfCell:(UITableView *)tableView indexPath:(NSIndexPath *)indexPath andBottomSpace:(CGFloat)space {
    
    NSAssert([tableView.dataSource respondsToSelector:@selector(tableView:cellForRowAtIndexPath:)], @"请实现 tableView:cellForRowAtIndexPath: 方法");
    
    // 获取cell
    UITableViewCell *cell = [tableView.dataSource tableView:tableView cellForRowAtIndexPath:indexPath];
    
    CGRect cellFrame = cell.frame;
    cellFrame.size.width = CGRectGetWidth(tableView.frame);
    cell.frame = cellFrame;
    
    [cell layoutIfNeeded];
    
    NSMutableArray *maxArray = [NSMutableArray array];
    for (UIView *view in cell.contentView.subviews) {
        
        [maxArray addObject:@(CGRectGetMaxY(view.frame))];
    }
    // 排序方法，从小到大的排序
    NSArray *compareMaxYArray = [maxArray sortedArrayUsingSelector:@selector(compare:)];

    return [[compareMaxYArray lastObject] floatValue] + space;
}
```

**<a href="http://zhidao.baidu.com/link?url=0Bp88lKSfF_yq_nwVPqmjSE-3yuAUGDRsGtKER15yTY780KAcQwhyJyMzaetAxbZFQox0oSoMER6C9qMQdYAi1f4-dbL3nvR8i9qTDYa6sa" target="_blank">uitableview 怎样获取指定分割线 并且设置颜色</a><br>**

```objc
UITableView * tableView = [[UITableView alloc] initWithFrame:CGRectMake(20, 20, 400, 300) style:UITableViewStylePlain];
 
    tableView.separatorColor = [UIColor redColor]; // 设置线颜色
 
    tableView.separatorInset = UIEdgeInsetsMake(0,80, 0, 80);        // 设置端距，这里表示separator离左边和右边均80像素
 
    tableView.separatorStyle = UITableViewCellSeparatorStyleSingleLine;
 
    tableView.dataSource = self;
```

**<a href="http://mobile.51cto.com/hot-404900.htm" target="_blank">OS开发UITableViewCell的选中时的颜色设置</a><br>**

**更新tableView只指定行的数据方法**

```objc
NSIndexPath *indexPath = [NSIndexPath indexPathForRow:row inSection:0];
[self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];
```

**更新tableView只指定section的数据方法**


```objc
// 刷新点击的组标题
[self.table reloadSections:[NSIndexSet indexSetWithIndex:view1.tag] withRowAnimation:UITableViewRowAnimationFade];
```

**tableView默认滚动到某一行/选中某一行**

```objc
NSIndexPath *indexpath = [NSIndexPath indexPathForRow:self.selectNumberOfRow inSection:0];
    
[self.tableView scrollToRowAtIndexPath:indexpath atScrollPosition:UITableViewScrollPositionTop animated:YES]; //滚动到第5行
    
[self.tableView selectRowAtIndexPath:indexpath animated:YES scrollPosition:UITableViewScrollPositionMiddle];  //选中第5行
```

**快速用xib创建一个cell进行测试**

```objc
/**
     1. 创建一个cell直接附带创建xib
     2. 在创建tableView的地方直接添加代码
     [self.tableView registerNib:[UINib nibWithNibName:@"TestTableViewCell" bundle:nil] forCellReuseIdentifier:@"hehe"];
     3. 在cellForRow里边进行设置
     static NSString *cellIdentifier = @"hehe";
     TestTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier forIndexPath:indexPath];
     只要cell名、xib和上边的两项保持一致，即可放心大胆的进行拖控件
     */
```

 **tableview不是从indexPath.row=0开始显示，原因可能是cell高度没有设置好**

> `UITableViewController -> VCOne -> VCTwo` 这种三层继承，在push VCTwo的时候，VCTwo不能带有xib,否则会崩溃

**在tableView的外部用一个button快速遍历每个cell中控件中的数值**

```objc
for (int i = 0; i < 5; i ++) {
        NSIndexPath *indexPath = [NSIndexPath indexPathForRow:i inSection:0];
        TestTableViewCell *cell = (TestTableViewCell *)[self.tableView cellForRowAtIndexPath:indexPath];
        NSLog(@"%@",cell.textField.text);
        
}
```


#### UITableView的常用语句
---

##### 1.UITableView有两种样式：

```objc
[[UITableView alloc] initWithFrame:view.bounds style:UITableViewStylePlain];
[[UITableView alloc] initWithFrame:view.bounds style:UITableViewStyleGrouped];
```

##### 2.UITableView的结构：

> UITableView由头部，尾部，和中间一连串的单元格组成，UITableView的头部由tableHeaderView属性设置，尾部由tableFooterView属性设置，中间的行高可通过rowHeight属性设置

```objc
_listArray = [[UIFont familyNames] retain];//获取所有字体名称
    
    _tableView = [[UITableView alloc] initWithFrame:view.bounds style:UITableViewStylePlain];
    // 设置数据源
    _tableView.dataSource = self;
    // 设置代理
    _tableView.delegate = self;
    // 设置表视图cell的高度，统一的高度
    _tableView.rowHeight = 70;    // 默认44px
    // 设置表视图的背景
    UIImageView *backgroundView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"IMG_0410"]];
    _tableView.backgroundView = backgroundView;
    [backgroundView release];
    // 设置表视图的颜色
//    _tableView.backgroundColor = [UIColor yellowColor];
    // 设置表视图的分割线的颜色
//    _tableView.separatorColor = [UIColor purpleColor];
    // 设置表视图的分割线的风格
    _tableView.separatorStyle = UITableViewCellSeparatorStyleSingleLine;
    // 设置表视图的头部视图(headView 添加子视图)
    UIView *headerView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 80)];
    headerView.backgroundColor = [UIColor redColor];
    // 添加子视图
    UILabel *headText = [[UILabel alloc] initWithFrame:CGRectMake(60, 0, 200, 80)];
    headText.text = @"天晴朗,天晴朗天晴朗天晴朗!";
    headText.numberOfLines = 0;
    [headerView addSubview:headText];
    [headText release];
    _tableView.tableHeaderView = headerView; //设置头部
    [headerView release];
    // 设置表视图的尾部视图(footerView 添加子视图)    
    UIView *footerView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 80)];
    footerView.backgroundColor = [UIColor yellowColor];
    _tableView.tableFooterView = footerView;  //设置尾部
    [footerView release];
```

##### 3.UITableView的一些常用属性

```objc
//设置UITableView分割线风格
@property(nonatomic) UITableViewCellSeparatorStyle separatorStyle; 
//设置UITableView分割线颜色，默认为标准灰色
@property(nonatomic,retain) UIColor               *separatorColor;  
//设置UITableView的头部
@property(nonatomic,retain) UIView *tableHeaderView; 
//设置UITableView的尾部
@property(nonatomic,retain) UIView *tableFooterView; 
//设置UITableView的Cell的高度
@property (nonatomic)          CGFloat                     rowHeight;
//设置UITableView种section的头部的高度
@property (nonatomic)          CGFloat                     sectionHeaderHeight;
//设置UITableView种section的尾部的高度
@property (nonatomic)          CGFloat                     sectionFooterHeight;
//设置UITableView的背景
@property(nonatomic, readwrite, retain) UIView *backgroundView NS_AVAILABLE_IOS(3_2);
//设置UITableView是否可编辑，默认为no，不可编辑
@property(nonatomic,getter=isEditing) BOOL editing; 
- (void)setEditing:(BOOL)editing animated:(BOOL)animated;//方法带有动画效果
//当UITableView不在编辑时，cell是否可以选中，默认为yes
@property(nonatomic) BOOL allowsSelection NS_AVAILABLE_IOS(3_0);  
//当UITableView在编辑时，cell是否可以选中，默认为no
@property(nonatomic) BOOL allowsSelectionDuringEditing;    
//当UITableView不在编辑时，cell是否可以选中多个，默认为no                                
@property(nonatomic) BOOL allowsMultipleSelection NS_AVAILABLE_IOS(5_0);  
//当UITableView在编辑时，cell是否可以选中多个，默认为no
@property(nonatomic) BOOL allowsMultipleSelectionDuringEditing NS_AVAILABLE_IOS(5_0);
```

##### 4.UITableView的一些常用方法：

```objc
//整体刷新UITableView
- (void)reloadData; 

//指定一个cell，返回一个NSIndexPath，如果cell没有，返回nil
- (NSIndexPath *)indexPathForCell:(UITableViewCell *)cell; 
//指定一个范围，返回一组NSIndexPath，如果rect无效，返回nil
- (NSArray *)indexPathsForRowsInRect:(CGRect)rect; 
//指定一个NSIndexPath，返回一个cell
- (UITableViewCell *)cellForRowAtIndexPath:(NSIndexPath *)indexPath; 
//返回所有显示的cell
- (NSArray *)visibleCells;
//返回所有显示的cell的NSIndexPath
- (NSArray *)indexPathsForVisibleRows;
```

##### 5. UITableView的一些编辑方法：

```objc
//插入一个cell到指定的indexPaths位置，指定一个动画效果
- (void)insertRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
//删除indexPaths位置的cell，指定一个动画效果
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
//刷新indexPaths位置的cell，指定一个动画效果（tableView的局部刷新，一般用于cell的位置不改变，又不想刷新整个tableView时）
- (void)reloadRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation NS_AVAILABLE_IOS(3_0);
//移动indexPaths位置的cell，指定一个动画效果
- (void)moveRowAtIndexPath:(NSIndexPath *)indexPath toIndexPath:(NSIndexPath *)newIndexPath NS_AVAILABLE_IOS(5_0);
```

##### 6. UITableView数据源方法

```objc
//UITableView有多少个组
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
    return 1;//默认为1
}
//UITableView每组有多少条数据
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
{
    return [_listArray count];
} 

//创建一个cell
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *cellIdentifier = @"cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    if (cell == nil) {
        cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier] autorelease];
    //cell的四种样式
    //UITableViewCellStyleDefault,       只显示图片和标题
       //UITableViewCellStyleValue1,        显示图片，标题和子标题（子标题在右边）
       //UITableViewCellStyleValue2,        标题和子标题
       //UITableViewCellStyleSubtitle        显示图片，标题和子标题（子标题在下边）

    }
    // 指向其中一行
//    cell.textLabel.text = [self.listArray objectAtIndex:indexPath.row];//设置cell的标题
    cell.textLabel.textColor = [UIColor redColor];//设置标题字体颜色
    cell.textLabel.font = [UIFont fontWithName:fontName size:18];//设置标题字体大小
    cell.imageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@""]];//设置cell的图片
    cell.detailTextLabel = @"detailTextLabel"// 设置cell的子标题
    return cell;
    
}
```

```objc
//设置组头部的文字
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section; 
//设置组尾部的文字
- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section;
```

```objc
//指定cell是否可编辑
- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath;
//指定cell是否可移动
- (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath;
//提交编辑操作，重写此方法，自动实现cell左滑动删除功能
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath;
// 移动cell
- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)sourceIndexPath toIndexPath:(NSIndexPath *)destinationIndexPath;
```

```objc
//右边索引显示的内容
- (NSArray *)sectionIndexTitlesForTableView:(UITableView *)tableView
{
    return _keyArray;
} 
// 点击右边索引跳转到哪个index位置
- (NSInteger)tableView:(UITableView *)tableView sectionForSectionIndexTitle:(NSString *)title atIndex:(NSInteger)index
{
    return index;
}
```

#### UITalbeView常用的代理方法
---

```objc
//cell的行高
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath;
//组头部的高度
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section;
//组尾部的高度
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section;
//自定义组头部视图，此方法和数据源中设置头部标题的方法只能实现一个
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section;   // custom view for header. will be adjusted to default or specified header height
//自定义组尾部视图，此方法和数据源中设置尾部标题的方法只能实现一个
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section;  
//点击cell时调用
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
//取消点击cell时调用
- (void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath NS_AVAILABLE_IOS(3_0);
```

#### UITableViewCell的一些辅助功能
---

**cell选中的样式**

```objc
cell.selectionStyle = UITableViewCellSelectionStyleBlue;
```

**如果想选中后取消，在didSelectRowAtIndexPath方法中调用**


```objc
[tableView deselectRowAtIndexPath:indexPath animated:YES];或
[self performSelector:@selector(deselectRowAtIndexPath:animated:) withObject:indexPath afterDelay:.5];
```

**如果想在cell的右边出现选中状态或箭头可以设置下面的属性**

```objc
cell.accessoryType = UITableViewCellAccessoryCheckmark;
```

**cell根据文字的多少自适应高度**

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // wrong  UITableViewCell *cell = [tableView cellForRowAtIndexPath:indexPath];
    NSString *text = [_listArray objectAtIndex:indexPath.row];
    //320为文字显示的宽度，高度1000是随便写的，会自动根据文字的大小和宽度计算出高度
    CGSize size = [text sizeWithFont:[UIFont systemFontOfSize:14] constrainedToSize:CGSizeMake(320, 1000)];
    // +20是为了让每个cell之间有些间隔
    return size.height+20;
}
```

```objc
//这样写在IOS7.0以后 TableViewCell的分割线就不会往右挫15个像素点了
   UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:SimpleTableIdentifier];
[tableViewsetSeparatorInset:UIEdgeInsetsMake(0,0,0,0)];
```