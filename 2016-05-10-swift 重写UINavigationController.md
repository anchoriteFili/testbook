---
layout: post
#标题
title:  swift 重写UINavigationController
#时间配置
date:   2016-05-10 16:19:22 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}


```swift
import UIKit

class WMNavigationController: UINavigationController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        //返回按钮等按钮的属性设置
        let item = UIBarButtonItem.appearance()
        
        //普通状态
        let textAttrs = NSDictionary(objects: [UIColor.whiteColor(),UIFont.systemFontOfSize(15)], forKeys: [NSForegroundColorAttributeName,NSFontAttributeName])
        item.setTitleTextAttributes((textAttrs as! [String : AnyObject]),
                                    forState: UIControlState.Normal)
        
        //不可用状态的状态栏
        let disableTextAttrs = NSDictionary(objects: [UIColor.whiteColor(),UIFont.systemFontOfSize(15)], forKeys: [NSForegroundColorAttributeName,NSFontAttributeName])
        item.setTitleTextAttributes((disableTextAttrs as! [String : AnyObject]),
                                    forState: UIControlState.Disabled)
        
        //设置navigationbar中间的title颜色
        let bar = UINavigationBar.appearance()
        let textAttrs2 = NSDictionary(objects: [UIColor.whiteColor(),UIFont.systemFontOfSize(20)], forKeys: [NSForegroundColorAttributeName,NSFontAttributeName])
        bar.titleTextAttributes = (textAttrs2 as! [String : AnyObject])
        bar.setBackgroundImage(UIImage(named: "jianbian"), forBarMetrics: UIBarMetrics.Default)

        // Do any additional setup after loading the view.
    }
    
    //重写推送页面方法
    override func pushViewController(viewController: UIViewController, animated: Bool) {
        
        if self.viewControllers.count > 0 {
            viewController.hidesBottomBarWhenPushed = true
        }
        
        super.pushViewController(viewController, animated: true)
        
    }
    
    func back() {
        self.popViewControllerAnimated(true)
    }
    
    func more() {
        self.popToRootViewControllerAnimated(true)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    

    /*
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        // Get the new view controller using segue.destinationViewController.
        // Pass the selected object to the new view controller.
    }
    */

}
```