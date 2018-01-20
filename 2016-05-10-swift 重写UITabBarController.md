---
layout: post
#标题
title:  swift 重写UITabBarController
#时间配置
date:   2016-05-10 16:21:22 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}


```swift
import UIKit

class MainViewController: UITabBarController {
    
    var shukebaoVC: ShouKeBao?

    override func viewDidLoad() {
        super.viewDidLoad()
        
        //旅游圈
        shukebaoVC = ShouKeBao()
        self.addChildVC(shukebaoVC!, title: "旅游圈", image: "skb2", selectedImage: "skb")
        
        //找产品
        let SB = UIStoryboard.init(name: "FindProductNew", bundle: NSBundle.mainBundle())
        let FPVC = SB.instantiateViewControllerWithIdentifier("FindProductNewSB") as! FindProductNew
        FPVC.findProductType = FindProductType.FindProductTypeNomal
        self.addChildVC(FPVC, title: "找产品", image: "fenlei2", selectedImage: "fenlei")
        
        //理订单
        let liDingDan = NewOrder()
        self.addChildVC(liDingDan, title: "理订单", image: "lidingdan", selectedImage: "lidingdan2")
        
        //去赚钱
        let SSB = UIStoryboard.init(name: "SaleCenterSB", bundle: NSBundle.mainBundle())
        let SCVC = SSB.instantiateViewControllerWithIdentifier("SaleCenterControllerSB") as! SaleCenterController
        FPVC.findProductType = FindProductType.FindProductTypeNomal
        self.addChildVC(SCVC, title: "去赚钱", image: "y赚钱-未选", selectedImage: "y赚钱-选中")
        
        let meVC = NewMe()
        self.addChildVC(meVC, title: "我", image: "wo2", selectedImage: "wo")
        

        // Do any additional setup after loading the view.
    }
    
    /**
     添加子页面方法
     - parameter controller:    子页面
     - parameter title:         子页面标题
     - parameter image:         子页面块正常状态图标
     - parameter selectedImage: 子页面块选择状态图标
     */
    func addChildVC(controller: UIViewController, title: String, image: String, selectedImage: String) {
        
        controller.title = title
        controller.tabBarItem.image = UIImage(named: image)
        controller.tabBarItem.selectedImage = UIImage(named: selectedImage)
        
        let nav = WMNavigationController(rootViewController: controller)
        self.addChildViewController(nav)

    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}
```