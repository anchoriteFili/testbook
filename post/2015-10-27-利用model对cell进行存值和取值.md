---
layout: post
#标题
title:  利用model对cell进行存值和取值
#时间配置
date:   2015-10-27 16:45:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/model赋值取值功能测试.zip" target="_blank">model赋值取值功能测试.zip</a><br>



> 原理：这个model就相当于本地临时数据存储的库，cell和VC可以通用的库，在页面不消失的情况下，里边的数据可以随便的更改model可以用于接收网络接口传过来的数据，也可以接收本地的数据来传输到网络

##### 1. 将接口中的所有数据在接口方法中添加到model中，然后将model添加到model数组中(没有数据赋值为空，初始化，不然重用出问题)

```objc
//创建Model
//创建Model数组
@property (nonatomic,strong) NSMutableArray *modelArray; //model数组

#pragma mark modelArray懒加载
- (NSMutableArray *)modelArray {
    if (!_modelArray) {
        _modelArray = [NSMutableArray array];
    }
    return _modelArray;
}

for (int i = 0; i < 10; i ++) {
        
        //将数据中获取到的值存入model中，并添加到model数组中
        TableViewCellModel *model = [[TableViewCellModel alloc] init];
        
        model.reciveData = [NSString stringWithFormat:@"名字 %d",i];
        model.textFieldText = @"";  
        [self.modelArray addObject:model];
}
```

##### 2. 根据数组个数定义cell的行数并对cell中元素进行赋值

```objc
#pragma mark cell行数
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

    //设置cell的行数
    return self.modelArray.count;
}

#pragma mark cellForRow方法
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    TableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"cell" forIndexPath:indexPath];
    
    //传输数据
    [cell updateCellWithModel:[self.modelArray objectAtIndex:indexPath.row]];
    
    // Configure the cell...
    
    return cell;
}

#pragma mark 利用传过来的model，对cell中所有的元素进行赋值
- (void)updateCellWithModel:(TableViewCellModel *)model {
    self.model = model;
    
    self.reciveData.text = model.reciveData;
    self.textField.text = model.textFieldText;
    
}

#pragma mark 这个方法是cell滑动的时候调用
- (void)setSelected:(BOOL)selected animated:(BOOL)animated {
    [super setSelected:selected animated:animated];
    
    //cell中自定义的数值，滑动式赋值给model
    self.model.textFieldText = self.textField.text;

    // Configure the view for the selected state
}
```

##### 3. 在点击保存，点击撤销等等一切要突出这个编辑状态的方法中，都调用便利cell中数据的方法，将所有的数据进行保存

```objc
#pragma mark 遍历出cell总的数据方法
- (void)reciveDataWithModel:(TableViewCellModel *)model {
    
    model.textFieldText = self.textField.text;
}

#pragma mark 保存按钮点击事件
- (IBAction)click:(UIButton *)sender {
    
    for (int i = 0; i < self.modelArray.count; i ++) {

#pragma mark 直接获取对应位置的cell,对cell进行操作
        NSIndexPath *indexPath = [NSIndexPath indexPathForRow:i inSection:0];
        TableViewCell *cell = (TableViewCell *)[self.tableView cellForRowAtIndexPath:indexPath];
        [cell reciveDataWithModel:[self.modelArray objectAtIndex:i]];
    }
    
    [self send];
}

#pragma mark 在此处可进行网络数据传输
- (void)send {
    
    for (int i = 0; i < self.modelArray.count; i ++) {
        NSLog(@"textFieldText ======= %@",[[self.modelArray objectAtIndex:i] textFieldText]);
    }
    
}
```