---
layout: post
#WKWebView快速创建
title: WKWebView快速创建
#时间配置
date:   2017-12-19 13:39:00 +0800
#大类配置
categories: 快速创建
#小类配置
tag: swift
---

* content
{:toc}


#### 引用的链接
---------
- [WKWebView用法介绍_swift](https://www.jianshu.com/p/091bf2467d67)<br>
- [iOS 8 WkWebView 网页的配置和前进，后退，js 交互和进度条的加载](http://blog.csdn.net/zhoushuangjian511/article/details/50380657)<br>
- [git仓库](https://github.com/anchoriteFili/SwiftWKWebView)<br>

#### 代码示例
----------------

```swift
import UIKit
import WebKit

class ViewController: UIViewController, WKUIDelegate, WKNavigationDelegate,WKScriptMessageHandler {
    
    func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {
        print("message ****** \(message.body)")
    }
    
    var webViewOne: WKWebView!
    var contentController: WKUserContentController = WKUserContentController()
    
    override func loadView() {
        
        /* 动态加载并运行JS代码 */
        // JS代码片段 这里没什么用，最好只在页面加载后调用
        let jsStr = ""
        // 根据JS字符串初始化WKUserScript对象
        let userScript = WKUserScript(source: jsStr, injectionTime: .atDocumentEnd, forMainFrameOnly: true)
        let userContentController = WKUserContentController()
        userContentController.addUserScript(userScript)
        
        // 根据生成的WKUserScript对象，初始化WKWebViewConfiguration
        let webConfiguration = WKWebViewConfiguration()
        webConfiguration.userContentController = userContentController
        
        webViewOne = WKWebView(frame: .zero, configuration: webConfiguration)
        webViewOne.uiDelegate = self
        webViewOne.navigationDelegate = self
        
        // 用观察者添加进度条
        webViewOne.addObserver(self, forKeyPath: "estimatedProgress", options: .new, context: nil)
        
        self.view = webViewOne
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        // https://tianji.rong360.com/tianjiwapreport/login?data=nD%2BRaNTUBXKJrM1QPpIetIpWI2iU5DejAjUo%2FRXJHMv%2BW13DSPQahuoMWVB1WxLBSyAwG7bcxRVgUE003PjpCrXD5DFxfthePEMRS7jd%2F4Wc%2BkmOEondu1Ckyn9lG9hhYaBobcA%2BanEaWnih7OLhtRwm%2FNT8eXMhWwPlmNvix0SWHY%2F3%2FFwxqaA1qNtQCTt7iYprTYXRRvdSr5ToKleZwO1aFQYPTGhFqLXe2sW2fi6oKE%2FZKdVQ2ovlICIo7qUS
        
        let myURL = URL(string: "https://tianji.rong360.com/tianjiwapreport/login?data=nD%2BRaNTUBXKJrM1QPpIetIpWI2iU5DejAjUo%2FRXJHMv%2BW13DSPQahuoMWVB1WxLBSyAwG7bcxRVgUE003PjpCrXD5DFxfthePEMRS7jd%2F4Wc%2BkmOEondu1Ckyn9lG9hhYaBobcA%2BanEaWnih7OLhtRwm%2FNT8eXMhWwPlmNvix0SWHY%2F3%2FFwxqaA1qNtQCTt7iYprTYXRRvdSr5ToKleZwO1aFQYPTGhFqLXe2sW2fi6oKE%2FZKdVQ2ovlICIo7qUS")
        let myRequest = URLRequest(url: myURL!)
        webViewOne.load(myRequest)
        contentController.add(self, name: "callbackHandler")
        
    }
    
    /**
     * WKNavigationDelegate
     */
    /* 页面开始加载时调用 */
    func webView(_ webView: WKWebView, didStartProvisionalNavigation navigation: WKNavigation!) {
        print("页面开始加载")
    }
    
    /* 当内容开始返回时调用 */
    func webView(_ webView: WKWebView, didCommit navigation: WKNavigation!) {
        print("内容开始返回")
    }
    
    /* 页面加载完成后时调用 */
    func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
        
//        NSString *str = @"";
//
//        [webView stringByEvaluatingJavaScriptFromString:str];}
        
        print("页面加载结束 ******** \(webView.url?.absoluteString)")
    }
    
    /* 页面加载失败时调用 */
    func webView(_ webView: WKWebView, didFailProvisionalNavigation navigation: WKNavigation!, withError error: Error) {
        print("网页加载失败")
    }
    
    /* 接收到服务器跳转请求之后调用 */
    func webView(_ webView: WKWebView, didReceiveServerRedirectForProvisionalNavigation navigation: WKNavigation!) {
        print("收到服务器跳转请求之后调用")
    }
    
    /* 在收到响应后，决定是否跳转 */
    func webView(_ webView: WKWebView, decidePolicyFor navigationResponse: WKNavigationResponse, decisionHandler: @escaping (WKNavigationResponsePolicy) -> Void) {
        print("************** \(webView.url)")
        decisionHandler(WKNavigationResponsePolicy.allow)
    }
    
    /*
     * 这个是比较牛的方法，无论前进后退，都会调用这个方法
     */
    func webView(_ webView: WKWebView, decidePolicyFor navigationAction: WKNavigationAction, decisionHandler: @escaping (WKNavigationActionPolicy) -> Void) {
        
        // 可以通过此来修改相关内容
        let jsString = "var list = document.getElementsByClassName('icon-icon-arrowleft')[0];list.onclick=function(){alert('点击了返回事件')}"
        webView.evaluateJavaScript(jsString) { (any, error) in
        }
        
        
        //        print("***************** \(navigationAction.navigationType)")
        //        print(navigationAction)
        
        // webView.backForwardList.backList.count 这里存放的是能返回到的个数
        
        // 这里存储的是可返回列表
        print(webViewOne.backForwardList.backList.count)
        // 这个获得的是当前页面的url
        print(navigationAction.request.url!.absoluteString)
        decisionHandler(WKNavigationActionPolicy.allow)
    }

    
    /*
     * WKUIDelegate 使用
     */
    /* WKWebView创建初始化加载的一些设置 */
    func webView(_ webView: WKWebView, createWebViewWith configuration: WKWebViewConfiguration, for navigationAction: WKNavigationAction, windowFeatures: WKWindowFeatures) -> WKWebView? {
        print("************** 1")
        
        return webView
    }
    
    /* 与界面弹出提示框相关的代理方法 */
    /* 处理JS中的提示框，若不使用该方法，则提示无效 */
    func webView(_ webView: WKWebView, runJavaScriptAlertPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: @escaping () -> Void) {
        
        let alertVC = UIAlertController.init(title: "提示", message: message, preferredStyle: .alert)
        alertVC.addAction(UIAlertAction.init(title: "确定", style: .default, handler: { (action) in
            completionHandler()
        }))
        self.present(alertVC, animated: true, completion: nil)
    }
    
    /* 处理网页js中确认框，若不使用该方法，则确认框无效 */
    func webView(_ webView: WKWebView, runJavaScriptConfirmPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: @escaping (Bool) -> Void) {
        print("调用了确认框")
    }
    
    /* 处理网页js中的文本输入 */
    func webView(_ webView: WKWebView, runJavaScriptTextInputPanelWithPrompt prompt: String, defaultText: String?, initiatedByFrame frame: WKFrameInfo, completionHandler: @escaping (String?) -> Void) {
        print("处理网页js的文本输入")
    }
    
    @available(iOS 9.0, *)
    func webViewDidClose(_ webView: WKWebView) {
        print("关闭了")
    }
    
    
    /* JS 调用 Native APP
     * WKScriptMessageHandler
     * 这个协议中包含一个必须实现的方法，这个方法是提高APP与web端交互的关键，它可以直接将接收到的JS脚本转为Swift对象
     */
    
    /*
     * 观察者，进度条部分
     */
    override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
        
        if keyPath == "estimatedProgress" {
            if object as? WKWebView == webViewOne {
                print("进度条 ****** \(webViewOne.estimatedProgress)")
            }
        }
        
    }
    
    deinit {
        self.removeObserver(self, forKeyPath: "estimatedProgress")
    }
    

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```
