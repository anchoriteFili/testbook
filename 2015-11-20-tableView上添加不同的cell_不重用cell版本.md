---
layout: post
#标题
title:  tableView不同cell的添加_重用cell版本
#时间配置
date:   2015-11-20 18:33:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/table中不同cell的创建_不重用cell.zip" target="_blank">table中不同cell的创建_不重用cell.zip</a><br>


**创建模板cell**

```objc
#import "MotherCell.h"

@implementation MotherCell

- (void)awakeFromNib {
    // Initialization code
}

#pragma mark 初始化的时候直接添加编辑按钮
- (instancetype)init {
    
    if (self = [super init]) {
        self.button = [UIButton buttonWithType:UIButtonTypeCustom];
        
        self.button.frame = CGRectMake(0, 60, WIDTH, 60);
        self.button.backgroundColor = [UIColor blueColor];
        [self.button setTitle:@"编辑" forState:UIControlStateNormal];
        [self.button setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
        [self.button addTarget:self action:@selector(editTouchUp:) forControlEvents:UIControlEventTouchUpInside];
        [self.contentView addSubview:self.button];
        
    }
    return self;
}

#pragma mark cell中编辑按钮的点击事件，触发VC中代理，执行编辑按钮事件
- (void)editTouchUp:(id)sender {
    
    if (self.cellDelegate && [self.cellDelegate respondsToSelector:@selector(editTouchUpDelegate:)]) {
        [self.cellDelegate editTouchUpDelegate:(int)self.cellType];
    }
}

#pragma mark 更新状态，确定是否屏蔽编辑按钮
- (void)updateState:(BOOL)isEditable {
    if (isEditable) {
        [self setEditable];
    } else {
        [self setNoEditable];
    }
}

- (void)setEditable {
    
    self.button.hidden = NO;
}

- (void)setNoEditable {
    self.button.hidden = YES;
}


- (void)setSelected:(BOOL)selected animated:(BOOL)animated {
    [super setSelected:selected animated:animated];

    // Configure the view for the selected state
}

@end
```

```objc
#import "MotherCell.h"

@implementation MotherCell

- (void)awakeFromNib {
    // Initialization code
}

#pragma mark 初始化的时候直接添加编辑按钮
- (instancetype)init {
    
    if (self = [super init]) {
        self.button = [UIButton buttonWithType:UIButtonTypeCustom];
        
        self.button.frame = CGRectMake(0, 60, WIDTH, 60);
        self.button.backgroundColor = [UIColor blueColor];
        [self.button setTitle:@"编辑" forState:UIControlStateNormal];
        [self.button setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
        [self.button addTarget:self action:@selector(editTouchUp:) forControlEvents:UIControlEventTouchUpInside];
        [self.contentView addSubview:self.button];
        
    }
    return self;
}

#pragma mark cell中编辑按钮的点击事件，触发VC中代理，执行编辑按钮事件
- (void)editTouchUp:(id)sender {
    
    if (self.cellDelegate && [self.cellDelegate respondsToSelector:@selector(editTouchUpDelegate:)]) {
        [self.cellDelegate editTouchUpDelegate:(int)self.cellType];
    }
}

#pragma mark 更新状态，确定是否屏蔽编辑按钮
- (void)updateState:(BOOL)isEditable {
    if (isEditable) {
        [self setEditable];
    } else {
        [self setNoEditable];
    }
}

- (void)setEditable {
    
    self.button.hidden = NO;
}

- (void)setNoEditable {
    self.button.hidden = YES;
}


- (void)setSelected:(BOOL)selected animated:(BOOL)animated {
    [super setSelected:selected animated:animated];

    // Configure the view for the selected state
}

@end
```

**创建子类cell**

```objc
#import "MotherCell.h"

@interface ChildCellOne : MotherCell

@property (nonatomic,strong) CellModelOne *model;

- (id)initCellWithModel:(CellModelOne *)model;

@end

#import "ChildCellOne.h"

@implementation ChildCellOne

#pragma mark 由于不用重用cell，所以直接用init方法
- (id)initCellWithModel:(CellModelOne *)model {
    
    if (self = [super init]) {
        self.cellHeight = 224; //初始化高度
        self.cellType = kChildCellOne; //初始化类型
        self.model = model;
        [self createInterface]; //创建界面
    }
    return self;
}

#pragma mark 直接创建界面
- (void)createInterface {
    
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, WIDTH, 60)];
    label.backgroundColor = [UIColor grayColor];
    label.text = self.model.textOne;
    label.textColor = [UIColor redColor];
    [self.contentView addSubview:label];

}

@end
```

**VC中设置**

```objc
#import "ViewController.h"
#import "CellModel.h"
#import "ChildCellOne.h"
#import "ChildCellTwo.h"
#import "ChildCellThree.h"

@interface ViewController ()<UITableViewDataSource,UITableViewDelegate>

@property (nonatomic,strong) UITableView *tabelView; //全局tableView
@property (nonatomic,strong) NSMutableArray *cellArray; //添加model数组
@property (nonatomic,assign) BOOL isEdit; //是否进入编辑状态



@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    
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
    
#pragma mark 添加cellArray，直接将带有数据的cell添加进了数组
    CellModelOne *modelOne = [[CellModelOne alloc] init];
    modelOne.textOne = @"第一行";
    ChildCellOne *cellOne = [[ChildCellOne alloc] initCellWithModel:modelOne];
    [self.cellArray addObject:cellOne];
    
    CellModelTwo *modelTwo = [[CellModelTwo alloc] init];
    modelTwo.textTwo = @"第二行";
    ChildCellTwo *cellTwo = [[ChildCellTwo alloc] initCellWithModel:modelTwo];
    [self.cellArray addObject:cellTwo];
    
    CellModelThree *modelThree = [[CellModelThree alloc] init];
    modelThree.textThree = @"第三行";
    ChildCellThree *cellThree = [[ChildCellThree alloc] initCellWithModel:modelThree];
    [self.cellArray addObject:cellThree];

}

#pragma mark 设置每行的高度
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    MotherCell *cell = [self.cellArray objectAtIndex:indexPath.row];
    
    return cell.cellHeight;
}

#pragma mark 设置section的个数
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
    return 1;
}

#pragma mark 设置每个section的行数
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
{
    return self.cellArray.count;
}

#pragma mark cellForRow方法
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    MotherCell *cell = [self.cellArray objectAtIndex:indexPath.row];
    
    //设置编辑状态/代理/和选择时的类型
    [cell updateState:self.isEdit];
    cell.cellDelegate = self;
    cell.selectionStyle = UITableViewCellSelectionStyleDefault;
    
    //编辑状态的时候关闭点击效果
    if (self.isEdit) {
        cell.selectionStyle = UITableViewCellSelectionStyleNone;
    }
    
    return cell;
}

#pragma mark 添加点击cell的方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
    if (!self.isEdit) {
        MotherCell *cell = [self.cellArray objectAtIndex:indexPath.row];
        NSLog(@"cell.cellType ===== %ld",(long)cell.cellType);
    }
    
    //点击以后直接恢复正常状态
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}

- (NSMutableArray *)cellArray {
    if (!_cellArray) {
        _cellArray = [NSMutableArray array];
    }
    return _cellArray;
}

#pragma mark cell的代理方法，点击不同的行的编辑按钮对不同的行就行操作
- (void)editTouchUpDelegate:(ChildEnumCellType)cellType {
    
    if (cellType == kChildCellOne) {
        NSLog(@"第一个按钮");
    } else if (cellType == kChildCellTwo) {
        NSLog(@"第二个按钮");
    } else if (cellType == kChildCellThree) {
        NSLog(@"第三个按钮");
    }
}

#pragma mark 编辑按钮的实现
- (IBAction)editButton:(UIButton *)sender {
    
    self.isEdit = !self.isEdit;
    [self.tabelView reloadData];
}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```