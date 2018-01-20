---
layout: post
#标题
title:  swift中VC页面跳转的动画效果-push和pop时的动画效果
#时间配置
date:   2016-05-06 15:10:22 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}

![752372-20160506140551404-1611089788.png]({{ site.img_url }}AD24619955361E1A1DE44AC7D433179E.png)

<a href="http://www.cnblogs.com/pengyingh/articles/2378732.html" target="_blank">CGAffineTransform相关函数</a><br>

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/页面动画的跳转.zip" target="_blank">页面动画的跳转.zip</a><br>

**相关代码**


```swift
import UIKit

class ViewController: UIViewController,UINavigationControllerDelegate,UIViewControllerAnimatedTransitioning,UIViewControllerInteractiveTransitioning {
    
    /**
     UINavigationControllerOperation 表示navigation的过度效果，是一个枚举值
     push 推送效果 pop 效果 none 没有效果
     */
    var navigationOperation: UINavigationControllerOperation? //过度效果
    
    weak var transitionContext: UIViewControllerContextTransitioning?
    weak var transitingView: UIView?
    var isTransiting: Bool = false
    
    override func viewDidLoad() {
        super.viewDidLoad()

        self.navigationController!.delegate = self
        self.navigationController!.view.backgroundColor = UIColor.whiteColor()
        
        //添加了手势？
        let popRecognizer = UIScreenEdgePanGestureRecognizer(target: self, action: #selector(ViewController.handlePopRecognizer(_:)))
        popRecognizer.edges = UIRectEdge.Left
        self.navigationController?.view.addGestureRecognizer(popRecognizer)
    }
    
    /**
     手势方法
     - parameter popRecognizer: 手势
     */
    func handlePopRecognizer(popRecognizer: UIScreenEdgePanGestureRecognizer) {
        
        var progress = popRecognizer.translationInView(navigationController?.view).x/(navigationController?.view.bounds.size.width)!
        progress = min(1.0, max(0.0, progress))
        //progress代表的是手势的x方向移动的数值与整个平宽的比例，如果移动超过一半，则退出页面
        print(progress)
        
        if popRecognizer.state == UIGestureRecognizerState.Began {
            isTransiting = true //这个重点，添加true时表示可以动画处理
            self.navigationController!.popViewControllerAnimated(true) //添加可实现动画效果
        } else if popRecognizer.state == UIGestureRecognizerState.Changed {
            updateWithPercent(progress)
            print("change")
        } else if popRecognizer.state == UIGestureRecognizerState.Ended || popRecognizer.state == UIGestureRecognizerState.Cancelled {
            
            finishBy(progress < 0.5) //当缩小到0.5的时候直接退出
            print("Ended || Cancelled")
            isTransiting = false
        }
    }
    
    /**
     根据数据转换
     - parameter percent: pi
     */
    func updateWithPercent(percent: CGFloat) {
        //转化成CGFloat方法CGFloat()，括号了放入值即可 fabsf()是用来转换格式时估算值的
        let scale = CGFloat(fabsf(Float(percent - CGFloat(1.0)))) //各种转换格式
        transitingView?.transform = CGAffineTransformMakeScale(scale, scale)
        transitionContext?.updateInteractiveTransition(percent)
    }
    
    //手势结束，退出页面时调用的方法
    func finishBy(cancelled: Bool) {
        if cancelled {
            UIView.animateWithDuration(0.4, animations: { 
                self.transitingView?.transform = CGAffineTransformIdentity
                }, completion: { (completed) in
                    self.transitionContext?.cancelInteractiveTransition()
                    self.transitionContext?.completeTransition(false)
            })
        } else {
            
            UIView.animateWithDuration(0.4, animations: { 
                print(self.transitingView)
                self.transitingView?.transform = CGAffineTransformMakeScale(0, 0)
                print(self.transitingView)
                }, completion: { (completed) in
                    self.transitionContext?.finishInteractiveTransition()
                    self.transitionContext?.completeTransition(true)
            })
        }
    }
    
    //这个家伙貌似一直没动
    func startInteractiveTransition(transitionContext: UIViewControllerContextTransitioning) {
        self.transitionContext = transitionContext
        
        let containerView = transitionContext.containerView()
        let toViewController = transitionContext.viewControllerForKey(UITransitionContextToViewControllerKey)
        let fromViewController = transitionContext.viewControllerForKey(UITransitionContextFromViewControllerKey)
        containerView!.insertSubview((toViewController?.view)!, belowSubview: (fromViewController?.view)!)
        self.transitingView = fromViewController?.view
    }
    
    // UINavigationControllerDelegate
    func navigationController(navigationController: UINavigationController, animationControllerForOperation operation: UINavigationControllerOperation, fromViewController fromVC: UIViewController, toViewController toVC: UIViewController) -> UIViewControllerAnimatedTransitioning? {
        
        navigationOperation = operation
        return self
    }
    
    /**
     用于判断交互动画的代理方法，如果不返回self，则手势控制动画就没用什么用了
     - parameter navigationController: 导航控制器
     - parameter animationController:  动画页面
     - returns: 返回的是参与交互的控制器，返回空页面不交互
     */
    func navigationController(navigationController: UINavigationController, interactionControllerForAnimationController animationController: UIViewControllerAnimatedTransitioning) -> UIViewControllerInteractiveTransitioning? {
        if !self.isTransiting {
            return nil
        }
        return self
    }
    
    //UIViewControllerTransitioningDelegate 设置过渡时间
    func transitionDuration(transitionContext: UIViewControllerContextTransitioning?) -> NSTimeInterval {
        return 0.5
    }
    
    /**
     过度的动画添加方法，里边用于添加矩阵等，用于页面的旋转等各种动画
     - parameter transitionContext: 代理接收到的上下文，对上下文进行设置创建动画效果
     */
    func animateTransition(transitionContext: UIViewControllerContextTransitioning) {
        
        let containerView = transitionContext.containerView()
        let toViewController = transitionContext.viewControllerForKey(UITransitionContextToViewControllerKey)
        let fromViewController = transitionContext.viewControllerForKey(UITransitionContextFromViewControllerKey)
        var destView: UIView!
        var destTransform: CGAffineTransform! //。。。矩阵
        
        //推得时候的动画推送效果
        if navigationOperation == UINavigationControllerOperation.Push {
            containerView?.insertSubview(toViewController!.view, aboveSubview: fromViewController!.view)
            destView = toViewController!.view
            destView.transform = CGAffineTransformMakeScale(1, 0.1)
            destTransform = CGAffineTransformMakeScale(1, 1)
            
        } else if navigationOperation == UINavigationControllerOperation.Pop {
            
            containerView!.insertSubview(toViewController!.view, belowSubview: fromViewController!.view)
            
            destView = fromViewController!.view
            destTransform = CGAffineTransformMakeScale(0.1, 1)
//            destTransform = CGAffineTransformMakeRotation(CGFloat(M_PI))
//            destTransform = CGAffineTransformScale(destTransform, 0.1, 1)
//            destTransform = CGAffineTransformRotate(destTransform, CGFloat(M_PI))
        }
        
        
        UIView.animateWithDuration(transitionDuration(transitionContext), animations: {
            destView.transform = destTransform
            }, completion: ({completed in
                if transitionContext.transitionWasCancelled() {
                    //                    destView.removeFromSuperview()
                }
                //告诉系统你的动画过程已经结束，这是非常重要的方法，必须调用。
                transitionContext.completeTransition(!transitionContext.transitionWasCancelled())
            }))
        
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}

```