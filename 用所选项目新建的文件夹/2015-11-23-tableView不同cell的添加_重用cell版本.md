---
layout: post
#标题
title:  tableView不同cell的添加_重用cell版本
#时间配置
date:   2015-11-23 14:17:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/tableView不同cell的添加二_model确定cell类型.zip" target="_blank">tableView不同cell的添加二_model确定cell类型.zip</a><br>

```objc
/**
     1. 创建一个Model，在其中创建枚举值对不同的数据进行标记，用于后面添加到不同的cell中
     2. 在Model类中分区域对不同cell的信息存储
     3. 在VC中，将数据添加到model中，然后加入到model数组中，注意，每次赋值前都要进行model初始化
     4. 根据数组喝model中定义的不同的cell类型将值赋值到不同的cell中
     */
```

##### 1. 创建model

```objc
#import <Foundation/Foundation.h>

typedef NS_ENUM(NSInteger, ChildCellName) {
    
    kChildCellOne = 0,
    kChildCellTwo,
    kChildCellThree
};

@interface Model : NSObject

@property (nonatomic,assign) float cellHeight;
@property (nonatomic,assign) ChildCellName cellEnum;

#pragma mark cellOne中的参数
@property (nonatomic,strong) NSString *labelText;

#pragma mark cellTwo中的参数
@property (nonatomic,strong) NSString *fieldText;

#pragma mark cellThree中的参数
@property (nonatomic,strong) NSString *buttonText;


@end
```

##### 2. 创建模板cell,添加所有子cell中共有的数据

```objc
#import <UIKit/UIKit.h>
#import "Model.h"

@interface MotherCell : UITableViewCell

/**
 如果所有的cell有一个或多个共同的控件/共同的方法/共同的元素，可以添加模板cell
 模板cell可以添加
 */

@property (nonatomic,strong) UILabel *label;

@property (nonatomic,strong) UITextField *textField;

@property (nonatomic,strong) UIButton *button;


- (void)updateCellWithModel:(Model *)model;


@end
```

##### 3. 在不同的子cell中添加自己的元素 

```objc
#import "ChildCellOne.h"

@implementation ChildCellOne

- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier {
    self = [super initWithStyle:style reuseIdentifier:reuseIdentifier];
    if (self) {
        
        self.label = [[UILabel alloc] initWithFrame:CGRectMake(0, 10, 50, 30)];
        self.label.textColor = [UIColor redColor];
        self.label.textAlignment = NSTextAlignmentCenter;
        [self addSubview:self.label];
    }
    return self;
}

- (void)updateCellWithModel:(Model *)model {
    
    self.label.text = model.labelText;
    model.cellHeight = 60;
}

@end
```

##### 4. 添加VC中数据

```objc
#import "ViewController.h"
#import "ChildCellOne.h"
#import "ChildCellTwo.h"
#import "ChildCellThree.h"

@interface ViewController ()<UITableViewDataSource,UITableViewDelegate>

@property (nonatomic,strong) UITableView *tabelView; //全局tableView

@property (nonatomic,strong) NSMutableArray *modelArray; //装载cell中的数据

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    /**
     1. 创建一个Model，在其中创建枚举值对不同的数据进行标记，用于后面添加到不同的cell中
     2. 在Model类中分区域对不同cell的信息存储
     3. 在VC中，将数据添加到model中，然后加入到model数组中，注意，每次赋值前都要进行model初始化
     4. 根据数组喝model中定义的不同的cell类型将值赋值到不同的cell中
     */
    
#pragma mark 创建tableView
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
    
    
#pragma mark 向modelArray中添加元素 将不同的数据存储到不同的类型中 记得每次都初始化
    Model *model = [[Model alloc] init];
    
    model.cellEnum = kChildCellOne;
    model.labelText = @"labelCell";
    [self.modelArray addObject:model];
    
    model = [[Model alloc] init];
    model.cellEnum = kChildCellTwo;
    model.fieldText = @"哈哈啊哈哈哈哈";
    [self.modelArray addObject:model];
    
    model = [[Model alloc] init];
    model.cellEnum = kChildCellTwo;
    model.fieldText = @"呵呵呵呵呵呵呵呵";
    [self.modelArray addObject:model];
    
    model = [[Model alloc] init];
    model.cellEnum = kChildCellThree;
    model.buttonText = @"按钮";
    [self.modelArray addObject:model];
    
    
}

#pragma mark 设置每行的高度
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    Model *model = [self.modelArray objectAtIndex:indexPath.row];
    
    return model.cellHeight;
}

#pragma mark 设置section的个数
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
    return 1;
}

#pragma mark 设置每个section的行数
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
{
    return self.modelArray.count;
}

#pragma mark cellForRow方法 根据不同的model类型将数据添加到不同的cell中
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *cellIdentifier = @"cell";
    
    Model *model = [self.modelArray objectAtIndex:indexPath.row];
    
    if (model.cellEnum == kChildCellOne) {
        
        ChildCellOne *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
        if (cell == nil) {
            cell = [[ChildCellOne alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
        }
        
        NSLog(@"cellOne");
        
        [cell updateCellWithModel:model];
        
        //    设置选择时的状态
        cell.selectionStyle = UITableViewCellSelectionStyleDefault;
        //    设置cell背景色为透明色
        cell.backgroundColor = [UIColor clearColor];
        
        return cell;
        
    } else if (model.cellEnum == kChildCellTwo) {
        
        ChildCellTwo *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
        if (cell == nil) {
            cell = [[ChildCellTwo alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
        }
        
        NSLog(@"cellTwo");
        
        [cell updateCellWithModel:model];
        
        //    设置选择时的状态
        cell.selectionStyle = UITableViewCellSelectionStyleDefault;
        //    设置cell背景色为透明色
        cell.backgroundColor = [UIColor clearColor];
        
        return cell;
        
    } else if (model.cellEnum == kChildCellThree) {
        
        ChildCellThree *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
        if (cell == nil) {
            cell = [[ChildCellThree alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
        }
        
        NSLog(@"cellThree");
        
        [cell updateCellWithModel:model];
        
        //    设置选择时的状态
        cell.selectionStyle = UITableViewCellSelectionStyleDefault;
        //    设置cell背景色为透明色
        cell.backgroundColor = [UIColor clearColor];
        
        return cell;

    }
    
    return nil;
}

#pragma mark 添加点击cell的方法 根据不同的cell类型触发不同的cell方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
     Model *model = [self.modelArray objectAtIndex:indexPath.row];
    
    if (model.cellEnum == kChildCellOne) {
        NSLog(@"第一类cell");
        
    } else if (model.cellEnum == kChildCellTwo) {
       NSLog(@"第二类cell");
        
    } else if (model.cellEnum == kChildCellThree) {
        NSLog(@"第三类cell");
    }
    
    //点击以后直接恢复正常状态
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}

- (NSMutableArray *)modelArray {
    if (!_modelArray) {
        _modelArray = [NSMutableArray array];
    }
    
    return _modelArray;
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```