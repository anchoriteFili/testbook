---
layout: post
#标题
title:  点击section展开cell
#时间配置
date:   2015-11-12 16:31:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/模拟点击Section展开cell方法.zip" target="_blank">模拟点击Section展开cell方法.zip</a><br>


**创建mainModel，用于接收每个section中的信息**

```objc
#import <Foundation/Foundation.h>

@interface MainModel : NSObject

@property (nonatomic,strong) NSString *sectionName; //section的名字
@property (nonatomic,assign) BOOL show; //是否显示

@property (nonatomic,strong) NSMutableArray *cellsArray; //cell个数

@end
```

**创建childModel，用于接收每个row中的信息**

```objc
#import <Foundation/Foundation.h>

@interface ChildModel : NSObject

@property (nonatomic,strong) NSString *Name; //名字

@end
```

**实现过程**

```objc
#import "ViewController.h"
#import "MainModel.h"
#import "ChildModel.h"

@interface ViewController ()<UITableViewDataSource,UITableViewDelegate>

@property (nonatomic,strong) NSMutableArray *modelArray; //mainModel数组
@property (nonatomic,strong) UITableView *tableView;

@property (nonatomic,assign) BOOL show; //是否显示

@end

@implementation ViewController

/**
 传值的逻辑：
 1. 创建一个sectionArray，用于存储所有的section
 2. 创建一个section的model，存储section的基本信息
 3. 创建一个cell的model，存储cell的基本信息
 4. 在section的model中添加存储cell中的model的数组
 5. 进行传值
 */

- (void)loadData {
    
    self.modelArray = [NSMutableArray array];
    
    for (int i = 0; i < 5; i ++) {
        
        MainModel *mainModel = [[MainModel alloc] init];
        mainModel.cellsArray = [NSMutableArray array];
        mainModel.sectionName = [NSString stringWithFormat:@"天火%d",i];
        
        for (int k = 0; k < 10; k ++) {
            ChildModel *childModel = [[ChildModel alloc] init];
            childModel.Name = [NSString stringWithFormat:@"天火%d0%d",i,k];
            [mainModel.cellsArray addObject:childModel];
        }
        [self.modelArray addObject:mainModel];
    }
    
    [self.tableView reloadData];
}

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
#pragma mark 创建tableView 
    self.tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height) style:UITableViewStylePlain];
    
    self.tableView.delegate = self;
    self.tableView.dataSource = self;
    //    设置tableView背景色为透明色
    self.tableView.backgroundColor = [UIColor clearColor];
    //    分割线样式
    //    tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
    [self.view addSubview:self.tableView];
    
    [self loadData];
}

#pragma mark 设置每行的高度
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return 60;
}

#pragma mark 根据modelArray的个数设置section的个数
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return self.modelArray.count;
}

#pragma mark 设置每个section的行数（根据section可以取得modelArray中指定的model，可对其内部进行修改）
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
{
    
    //如果显示的话就返回行数为真正行数，不显示返回0
    MainModel *mainModel = [self.modelArray objectAtIndex:section];
    if (mainModel.show) {
        return [[[self.modelArray objectAtIndex:section] cellsArray] count];
    } else {
        return 0;
    }
}

#pragma mark 编写section的header，定义上面的元素
-(UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section {
    
    UIView *view = [[UIView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, 30)];
    
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    button.frame = CGRectMake(0, 0, 300, 30);
    [button setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
    NSString *title = [[self.modelArray objectAtIndex:section] sectionName];
    [button setTitle:title forState:UIControlStateNormal];
    button.tag = section+1000; //通过tag值来传输section数据
    [button addTarget:self action:@selector(buttonClick:) forControlEvents:UIControlEventTouchUpInside];
    [view addSubview:button];
    return view;
}

#pragma mark 设置section的header的高度
-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section {

    return 30;
}

#pragma mark cellForRow方法
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    
    static NSString *cellIdentifier = @"cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    [tableView setSeparatorInset:UIEdgeInsetsMake(0,0,0,0)];
    if (cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
    }
    
    //    设置选择时的状态
    cell.selectionStyle = UITableViewCellSelectionStyleDefault;
    //    设置cell背景色为透明色
    cell.backgroundColor = [UIColor clearColor];
    
#pragma mark 先通过section在modelArray中获取需要的mainModel，在通过row获取mainModel中的指定行的信息
    MainModel *mainModel = [self.modelArray objectAtIndex:indexPath.section];
    ChildModel *childModel = [mainModel.cellsArray objectAtIndex:indexPath.row];
    cell.textLabel.text = childModel.Name;
    
    return cell;
}

#pragma mark 展开和关闭的点击事件，
- (void)buttonClick:(UIButton *)sender {
    
    MainModel *mainModel = [self.modelArray objectAtIndex:(sender.tag-1000)];
    mainModel.show = !mainModel.show;
    
    // 刷新点击的组标题
    [self.tableView reloadSections:[NSIndexSet indexSetWithIndex:(sender.tag-1000)] withRowAnimation:UITableViewRowAnimationFade];
}


#pragma mark 添加点击cell的方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
    //点击直接恢复常态
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```