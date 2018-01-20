---
layout: post
#swift 使用xib创建view
title:  swift 使用xib创建view
#时间配置
date:   2017-12-11 18:24:16 +0800
#大类配置
categories: 快速创建
#小类配置
tag: swift
---

* content
{:toc}

**参考链接：**[swift用xib 自定义View](http://blog.csdn.net/yeshennet/article/details/51577213)

##### 1> 创建一个view以及同名的xib

![](/styles/images/resources/7BAE6377C5C1AA02132BC3CC0594F910.png)

##### 2> 在view中添加下边相关代码，完成与xib的链接

```swift
// 初始化方法
static func newInstance() -> NetStateView?{
        let nibView = Bundle.main.loadNibNamed("NetStateView", owner: nil, options: nil)
        if let view = nibView?.first as? NetStateView{
            return view
        }
        return nil
    }
    
    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        load_init()
    }
    
    func load_init(){
    }
```

##### 3> 使用初始化方法创建view 

```swift
lazy var netStateView: NetStateView = {
        () -> NetStateView in
        let netStateView = NetStateView.newInstance()
        
        if isRootViewController && !IsIphoneX {
            netStateView?.frame = CGRect(x: 0, y: 0, width: WIDTH, height: HEIGHT)
        } else {
            netStateView?.frame = CGRect(x: 0, y: 0, width: WIDTH, height: HEIGHT-Navigation_StatusHeight)
        }
        return netStateView!
    }()
```
