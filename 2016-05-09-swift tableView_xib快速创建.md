---
layout: post
#标题
title:  swift tableView_xib快速创建
#时间配置
date:   2016-05-09 17:49:22 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}

<a href="http://my.oschina.net/u/237983/blog/289222" target="_blank">Swift_ uitableview使用自定义cell</a><br>

**快捷添加**

```swift
let initIdentifier = "ChildAccountVCCell" //cell重用标识(全局)
// UITableViewDelegate,UITableViewDataSource
        self.tableView.delegate = self
        self.tableView.dataSource = self
        
        self.tableView.registerNib(UINib.init(nibName: "ChildAccountVCCell", bundle: nil), forCellReuseIdentifier: initIdentifier)

/**
     tableView部分
     */
    func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 1
    }
    
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 10
    }
    
    func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat {
        return 93
    }
    
    //cellForRow方法
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        
        let mycell : ChildAccountVCCell = tableView.dequeueReusableCellWithIdentifier(initIdentifier, forIndexPath: indexPath) as! ChildAccountVCCell
        
        
        return mycell
        
    }
    
    //cell点击事件
    func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
        
        //自动解除点击状态
        tableView.deselectRowAtIndexPath(indexPath, animated: true)
    }
```