---
layout: post
#标题
title:  CollectionView快速创建
#时间配置
date:   2015-05-11 20:08:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

```objc
#pragma 配置文件
    [self.CollectioinView registerClass:[SHStoreCollectionCell class] forCellWithReuseIdentifier:@"storeCollectionCell"];

- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView {
    return 1;
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    
    return 4;
    
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    
    static  NSString * identifierCell = @"storeCollectionCell";
    SHStoreCollectionCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:identifierCell forIndexPath:indexPath];
    
    cell.backgroundColor = [UIColor redColor];
    
    return cell;
}
```

**添加Identifier**

![752372-20160331214635113-1975658676.png]({{ site.img_url }}31478A0B24DC1C70D479F04D03644426.png)

**在cell中添加初始化方法，添加xib，否则xib不显示**

**UICollectionView简单的说可以把它理解成多列的UITableView。**

![112000247988559.png]({{ site.img_url }}FF4F012E36D3ABBC7B7B20A00C2644ED.png)

> 每一个格子都是代表一个cell,一行可以有多个cell,在collectionView中，cell的布局比tableView复杂，需要使用一个类描述集合视图的布局和行——UICollectionViewLayout

![112002032987600.png]({{ site.img_url }}104243165395F34D0079D01C83A09FA2.png)

**代码实现**

```objc
#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
//    1.创建布局管理类---flowLayout:流式布局
    UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
    
//    1.1设置滚动方向
//    layout.scrollDirection = UICollectionViewScrollDirectionHorizontal;
    layout.scrollDirection = UICollectionViewScrollDirectionVertical;
//    设置section内部cell之间的距离和大小
//    1.2设置竖直方向cell之间距离
    layout.minimumInteritemSpacing = 20;
//    1.3设置行方向cell之间距离
    layout.minimumLineSpacing = 10;
//    1.4设置cell的大小
    layout.itemSize = CGSizeMake(70, 70);
//    1.5设置整个section的上下左右的距离
    layout.sectionInset = UIEdgeInsetsMake(20, 30, 50, 20);
//    1.6
    layout.headerReferenceSize = CGSizeMake(320, 10);
   
//    2.根据布局管理类创建collectionView
    UICollectionView *collectionView = [[UICollectionView alloc] initWithFrame:self.view.bounds collectionViewLayout:layout];
    
//    3.设置数据源，代理
    collectionView.delegate = self;
    collectionView.dataSource = self;
    
//    4.注册cell类，当collectionView无法从重用队列中取得cell时，会根据此注册类创建cell---没有cell创建cell,不写崩溃
//    创建自定义cell--CustomCell
    [collectionView registerClass:[CustomCell class] forCellWithReuseIdentifier:@"myCell"];
    
//    5.注册增补视图
//    增补视图:视图的头和尾
//    创建自定义头view--ReusableView
    [collectionView registerClass:[ReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"reuserHeader"];
    
    [self.view addSubview:collectionView];
    [collectionView release];
    [layout release];
    
}

- (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath {
    ReusableView *reuserableView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"reuserHeader" forIndexPath:indexPath];
    
    if (indexPath.section == 0) {
        reuserableView.titleLable.text = @"A";
    }else {
        reuserableView.titleLable.text = @"B";
    }
    
    return reuserableView;
}

- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView {
    return 2;
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    
    return 10;
    
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    
    CustomCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"myCell" forIndexPath:indexPath];
    
    cell.backgroundColor = [UIColor redColor];
    
    return cell;
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```