---
layout: post
#标题
title: collectionView的点击展开
#时间配置
date:   2015-12-09 10:46:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![752372-20151209104412574-2120506778.gif]({{ site.img_url }}730369BEE201448326E299D9B9A3FF3B.gif)

**<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/collectionView的展开.zip" target="_blank">collectionView的展开.zip</a><br>**

**基本代码展示**

```objc
//
//  ViewController.m
//  项目众筹中部模拟
//
//  Created by 哈哈 on 15/12/2.
//  Copyright © 2015年 哈哈. All rights reserved.
//

#import "ViewController.h"
#import "Header.h"

@interface ViewController ()

@property (nonatomic,strong) UIView *topView; //更多众筹详情进度注资人开放View
@property (nonatomic,strong) UIImageView *topImageView; //更多众筹详情进度注资人图片
@property (nonatomic,strong) UIButton *topButton; //下拉按钮
@property (nonatomic,strong) UIView *bottomView; //下面的收回的按钮view
@property (nonatomic,strong) UIImageView *bottomImageView; //收起图片
@property (nonatomic,strong) UIButton *bottomButton; //回收按钮


@property (nonatomic,strong) UIView *centerView; //众筹中部的注资人点击部分
@property (nonatomic,strong) UILabel *followerLabel; //意向跟头人label
@property (nonatomic,strong) UIView *followerLabelBackView; //
@property (nonatomic,strong) UICollectionView *centerCollectionView; //中间collectionView部分
@property (nonatomic,strong) UIView *LookAllFollowerView; //查看所有跟头人View
@property (nonatomic,strong) UIButton *LookAllFollowerButton; //查看所有跟头人按钮
@property (nonatomic,strong) UILabel *LookAllFollowerLabel; //查看全部跟头人label
@property (nonatomic,strong) UILabel *prospectusLabel; //商业计划书label
@property (nonatomic,strong) UIView *BPView; //创建BP部分
@property (nonatomic,strong) UIImageView *BPImageView; //BP图片部分
@property (nonatomic,strong) UIButton *BPButton; //BP按钮

@property (nonatomic,assign) BOOL isCenterView; //是否显示centerView
@property (nonatomic,assign) BOOL isAllFollower; //是否显示全部跟投人
@property (nonatomic,assign) int number; //跟投人数量

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    self.number = 22;
    
#pragma mark 创建更多众筹详情进队注资人开放View
    self.topView = [[UIView alloc] initWithFrame:CGRectMake(0, 69, WIDTH, 50)];
   
    self.topView.backgroundColor = [UIColor whiteColor];
    
    self.topImageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, 195, 28)];
    self.topImageView.center = CGPointMake(WIDTH/2, 25);
    self.topImageView.image = [UIImage imageNamed:@"更多众筹详情仅对认购人开放"];
    [self.topView addSubview:self.topImageView];
    
    self.topButton = [UIButton buttonWithType:UIButtonTypeCustom];
    self.topButton.frame = CGRectMake(0, 0, WIDTH, 50);
    [self.topButton addTarget:self action:@selector(moreFollwerButtonClick:) forControlEvents:UIControlEventTouchUpInside];
    [self.topView addSubview:self.topButton];
    
    [self.view addSubview:self.topView];

#pragma mark 中部view添加
    self.centerView = [[UIView alloc] initWithFrame:CGRectMake(0, 64, WIDTH, HEIGHT)];
    
#pragma mark 已认购投资人
    self.followerLabel = [[UILabel alloc] initWithFrame:CGRectMake(WIDTHADP*10, WIDTHADP*12, 65, 13)];
    self.followerLabel.textColor = RGB_COLOR(51, 51, 51);
    self.followerLabel.font = [UIFont systemFontOfSize:13];
    [self.centerView addSubview:self.followerLabel];
    int personNum = 20;
    
#pragma mark 单独出来的部分
    self.followerLabel.text = [NSString stringWithFormat:@"已认购跟头人(%d人)",personNum];
    self.followerLabel.frame = CGRectMake(WIDTHADP*10, WIDTHADP*12, 200, 13);

#pragma mark *************collectionView部分*************
    //    1.创建布局管理类---flowLayout:流式布局
    UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
    
    //    1.1设置滚动方向
    //    layout.scrollDirection = UICollectionViewScrollDirectionHorizontal;
    layout.scrollDirection = UICollectionViewScrollDirectionVertical;
    //    设置section内部cell之间的距离和大小
    //    1.2设置竖直cell之间距离
    layout.minimumInteritemSpacing = (WIDTH-270)/3.0;
    //    1.3设置行之间距离
    layout.minimumLineSpacing = 10;
    //    1.4设置cell的大小
    layout.itemSize = CGSizeMake(60, 77);
    //    1.5设置整个section的上下左右的距离
    layout.sectionInset = UIEdgeInsetsMake(0, 10, 10, 10);
    //    1.6
    layout.headerReferenceSize = CGSizeMake(320, 10);
    
    //    2.根据布局管理类创建collectionView
    self.centerCollectionView = [[UICollectionView alloc] initWithFrame:CGRectMake(0, GETY(self.followerLabel)+GETHEIGHT(self.followerLabel)+WIDTHADP*12, WIDTH, 184) collectionViewLayout:layout];
    
    self.centerCollectionView.backgroundColor = [UIColor whiteColor];
    
    //    3.设置数据源，代理
    self.centerCollectionView.delegate = self;
    self.centerCollectionView.dataSource = self;
    
    //    4.注册cell类，当collectionView无法从重用队列中取得cell时，会根据此注册类创建cell---没有cell创建cell,不写崩溃
    //    创建自定义cell--CustomCell
    [self.centerCollectionView registerClass:[MoreInfoCollectionCell class] forCellWithReuseIdentifier:@"MoreInfoCollectionCell"];

    [self.centerView addSubview:self.centerCollectionView];
#pragma mark *******************end*******************
    
#pragma mark 创建查看全部跟投人View
    self.LookAllFollowerView = [[UIView alloc] initWithFrame:CGRectMake(0, GETY(self.centerCollectionView)+GETHEIGHT(self.centerCollectionView), WIDTH, 40)];
    self.LookAllFollowerView.backgroundColor = [UIColor redColor];
    
#pragma mark 创建查看全部跟头人label
    self.LookAllFollowerLabel = [[UILabel alloc] init];
    self.LookAllFollowerLabel.text = [NSString stringWithFormat:@"查看全部跟投人(%d人)",personNum];
    self.LookAllFollowerLabel.textColor = RGB_COLOR(153, 153, 153);
    self.LookAllFollowerLabel.font = [UIFont systemFontOfSize:12];
    [self.LookAllFollowerLabel sizeToFit];
    self.LookAllFollowerLabel.center = CGPointMake(WIDTH/2, 20);
    [self.LookAllFollowerView addSubview:self.LookAllFollowerLabel];
    
#pragma mark 创建查看全部跟投人button
    self.LookAllFollowerButton = [UIButton buttonWithType:UIButtonTypeCustom];
    self.LookAllFollowerButton.frame = CGRectMake(0, 0, WIDTH, 40);
    [self.LookAllFollowerButton addTarget:self action:@selector(LookAllFollowerButtonClick:) forControlEvents:UIControlEventTouchUpInside];
    [self.LookAllFollowerView addSubview:self.LookAllFollowerButton];
    [self.centerView addSubview:self.LookAllFollowerView];
    
#pragma mark 创建商业计划书label
    self.prospectusLabel = [[UILabel alloc] initWithFrame:CGRectMake(WIDTHADP*10, GETY(self.LookAllFollowerView)+GETHEIGHT(self.LookAllFollowerView)+WIDTHADP*12, 65, 13)];
    self.prospectusLabel.textColor = RGB_COLOR(51, 51, 51);
    self.prospectusLabel.font = [UIFont systemFontOfSize:13];
    [self.centerView addSubview:self.prospectusLabel];
    
#pragma mark 单独出来的部分
    self.prospectusLabel.text = @"商业计划书";
    self.prospectusLabel.frame = CGRectMake(WIDTHADP*10, GETY(self.LookAllFollowerView)+GETHEIGHT(self.LookAllFollowerView)+WIDTHADP*12, 65, 13);
    
#pragma mark 创建BPview
    self.BPView = [[UIView alloc] init];
    
#pragma mark 创建BP图片
    self.BPImageView = [[UIImageView alloc] initWithFrame:CGRectMake(WIDTH/2-55/2.0, WIDTHADP*20, 55, 55)];
    self.BPImageView.image = [UIImage imageNamed:@"BP_icon"];
    [self.BPView addSubview:self.BPImageView];
    
#pragma mark 创建BP按钮
    self.BPButton = [UIButton buttonWithType:UIButtonTypeCustom];
    self.BPButton.frame = CGRectMake(WIDTH/2-150/2, GETY(self.BPImageView)+GETHEIGHT(self.BPImageView)+WIDTHADP*20, 150, 40);
    [self.BPButton setBackgroundImage:[UIImage imageNamed:@"查看bp"] forState:UIControlStateNormal];
    [self.BPButton setBackgroundImage:[UIImage imageNamed:@"查看bp-_点击"] forState:UIControlStateHighlighted];
    [self.BPButton addTarget:self action:@selector(BPClick:) forControlEvents:UIControlEventTouchUpInside];
    [self.BPView addSubview:self.BPButton];
    
    self.BPView.backgroundColor = [UIColor whiteColor];
    
    self.BPView.frame = CGRectMake(0, GETY(self.prospectusLabel)+GETHEIGHT(self.prospectusLabel)+WIDTHADP*12, WIDTH, GETHEIGHT(self.BPButton)+GETY(self.BPButton)+WIDTHADP*25);
    [self.centerView addSubview:self.BPView];
    
#pragma mark 创建上拉按钮
    self.bottomView = [[UIView alloc] initWithFrame:CGRectMake(0, GETY(self.BPView)+GETHEIGHT(self.BPView), WIDTH, 50)];
    
    self.bottomView.backgroundColor = [UIColor whiteColor];
    
    self.bottomImageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, 29, 28)];
    self.bottomImageView.center = CGPointMake(WIDTH/2, 25);
    self.bottomImageView.image = [UIImage imageNamed:@"收起"];
    [self.bottomView addSubview:self.bottomImageView];
    
    self.bottomButton = [UIButton buttonWithType:UIButtonTypeCustom];
    self.bottomButton.frame = CGRectMake(0, 0, WIDTH, 50);
    [self.bottomButton addTarget:self action:@selector(moreFollwerButtonClick:) forControlEvents:UIControlEventTouchUpInside];
    [self.bottomView addSubview:self.bottomButton];
    
    [self.centerView addSubview:self.bottomView];
    
    self.centerView.frame = CGRectMake(0, 64, WIDTH, GETY(self.bottomView)+GETHEIGHT(self.bottomView));
    
    [self.view addSubview:self.centerView];
    
#pragma mark 初始时隐藏centerView
    self.isCenterView = YES;
    self.centerView.hidden = YES;
}

- (void)moreFollwerButtonClick:(UIButton *)sender {
    NSLog(@"跟多信息");
    
    if (self.isCenterView) {
        
        self.centerView.hidden = NO;
        self.topView.hidden = YES;
        [self.view setNeedsDisplay];
        self.isCenterView = NO;
    } else {
        
        self.centerView.hidden = YES;
        self.topView.hidden = NO;
        [self.view setNeedsDisplay];
        self.isCenterView = YES;
        
    }
}


#pragma mark BP按钮点击事件
- (void)BPClick:(UIButton *)sender {
    NSLog(@"点击BP");
}

#pragma mark 查看全部跟投人点击事件,重写下面的view的frame
- (void)LookAllFollowerButtonClick:(UIButton *)sender {
    
    NSLog(@"查看全部跟投人");
    
#pragma mark 显示全部赋值YES
    self.isAllFollower = YES;
    
#pragma mark 重写collectionView的frame
    
    
    int items = self.number%4;
    if (items > 0) {
        self.centerCollectionView.frame = CGRectMake(0, GETY(self.followerLabel)+GETHEIGHT(self.followerLabel)+WIDTHADP*12, WIDTH, (10+(self.number/4+1)*87));
    } else {
        self.centerCollectionView.frame = CGRectMake(0, GETY(self.followerLabel)+GETHEIGHT(self.followerLabel)+WIDTHADP*12, WIDTH, (10+self.number/4*87));
    }
    
    
    
    
    //屏蔽查看全部跟投人点击按钮
    self.LookAllFollowerView.hidden = YES;
    //重写商业计划书frame
    self.prospectusLabel.frame = CGRectMake(WIDTHADP*10, GETY(self.centerCollectionView)+GETHEIGHT(self.centerCollectionView)+WIDTHADP*12, 65, 13);
    //重写BPview的fram
    self.BPView.frame = CGRectMake(0, GETY(self.prospectusLabel)+GETHEIGHT(self.prospectusLabel)+WIDTHADP*12, WIDTH, GETHEIGHT(self.BPButton)+GETY(self.BPButton)+WIDTHADP*25);
    //重写下面收起的fram
    self.bottomView.frame = CGRectMake(0, GETY(self.BPView)+GETHEIGHT(self.BPView), WIDTH, 50);
    
    //重写中间的centerView的frame
    self.centerView.frame = CGRectMake(0, 64, WIDTH, GETY(self.bottomView)+GETHEIGHT(self.bottomView));
    
    [self.centerCollectionView reloadData];
    
    [self.view setNeedsDisplay];
    
}

#pragma mark ******************collectionView代理区******************
- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView {
    return 1;
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    
    if (self.isAllFollower) {
        
        return self.number;
    }
    return 8;
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    
    MoreInfoCollectionCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"MoreInfoCollectionCell" forIndexPath:indexPath];
    
    
    [cell updateWithModel:nil];
    
    
    cell.backgroundColor = [UIColor redColor];
    
    return cell;
}

- (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath {
    
    NSLog(@"%ld",(long)indexPath.item);
    
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```