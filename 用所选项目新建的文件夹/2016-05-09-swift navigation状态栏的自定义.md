---
layout: post
#标题
title:  swift navigation状态栏的自定义
#时间配置
date:   2016-05-09 17:51:22 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}


```swift
/**
     自定义navigation
     */
    func setNavigationItem() {
        
        //添加导航栏右边+号按钮
        
        let rightButton = UIButton.init(frame: CGRectMake(0, 0, 18, 18))
        rightButton.setImage(UIImage(named: "add_bt"), forState: UIControlState.Normal)
        rightButton.addTarget(self, action: #selector(ChildAccountViewController.rightButtonClick(_:)), forControlEvents: UIControlEvents.TouchUpInside)
        self.navigationItem.rightBarButtonItem = UIBarButtonItem.init(customView: rightButton);
        
        //重写导航栏中部titleView
        centerButton = UIButton(type: UIButtonType.Custom)
        centerButton?.frame = CGRectMake(0, 0, 180, 30)
        
        centerButton?.setImage(UIImage(named: "whidexiala"), forState: UIControlState.Normal)
        centerButton?.imageEdgeInsets = UIEdgeInsetsMake(0, 160, 0, 10);
        centerButton?.setTitle("选择顾问登录", forState: UIControlState.Normal)
        centerButton?.titleEdgeInsets = UIEdgeInsetsMake(0,5, 0, 20);
        
        self.navigationItem.titleView = centerButton
    }
    
    func rightButtonClick(item: UIBarButtonItem) {
       
        print("有按钮点击事件")
        
    }
```