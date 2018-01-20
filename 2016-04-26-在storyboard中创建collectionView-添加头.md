---
layout: post
#标题
title:  在storyboard中创建collectionView-添加头
#时间配置
date:   2016-04-26 16:44:22 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![752372-20160426163749798-17047030.png]({{ site.img_url }}722FD307F1A676B673D05E5AD531C29E.png)

![752372-20160426164237080-462284572.png]({{ site.img_url }}896DD05B5CB448A67B80E2A14851FB69.png)

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/collectionView的模拟创建.zip" target="_blank">collectionView的模拟创建.zip</a><br>

```objc
#import "FindProductNew.h"
#import "SHStoreCollectionCell.h"
#import "HeaderSectionView.h"

@interface FindProductNew ()<UICollectionViewDelegate,UICollectionViewDataSource>


@property (weak, nonatomic) IBOutlet UICollectionView *rightCollection; //创建collection

@end

@implementation FindProductNew

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    
    self.view.backgroundColor = [UIColor yellowColor];
    
    
#pragma 配置文件
    self.rightCollection.delegate = self;
    self.rightCollection.dataSource = self;
#warning 在storyboard中，不能使用下面的代码，否则cell中的内容都不显示
//    [self.rightCollection registerClass:[SHStoreCollectionCell class] forCellWithReuseIdentifier:@"SHStoreCollectionCell"];
 }

- (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath {
    
    HeaderSectionView *headerView = [collectionView dequeueReusableSupplementaryViewOfKind:kind withReuseIdentifier:@"HeaderSectionView" forIndexPath:indexPath];
    return headerView;
}

- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView {
    return 2;
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    
    return 10;
    
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    
#pragma mark 在storyboard中重用identifier与这个要一致
    static  NSString * identifierCell = @"SHStoreCollectionCell";
    SHStoreCollectionCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:identifierCell forIndexPath:indexPath];
    
    cell.placeLabel.text = @"张家界";
    cell.backgroundColor = [UIColor whiteColor];
    
    return cell;
}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end

```