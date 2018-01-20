---
layout: post
#标题
title:  swift闭包与OC中Block循环引用[weak self]
#时间配置
date:   2018-01-08 15:18:08 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}


<a href="https://www.cnblogs.com/weijingyun/p/4625739.html" target="_blank">swift中闭包 OC中Block 解决循环引用</a><br>


**OC中全局宏定义**

```objc
#define WS(weakSelf)  __weak __typeof(&*self)weakSelf = self;

用法如下：

WS(weakself)

[self.tableView addHeaderWithCallback:^{

[weakself requestMemberList];

}];
```

**swift 在比闭包 中使用 weakSelf**

```swift
weak var weakSelf = self
demo4 {
    // 使用?的好处 就是一旦 self 被释放，就什么也不做
    weakSelf?.view.backgroundColor = UIColor.redColor()
}
```

**直接添加[weak self]**

```swift
HYHttpTool.post(url: urlString, param: ["appid":WXAPPID,"secret":WXSECRET,"code":code,"grant_type":"authorization_code",]) { [weak self] (response, result) in
  self?.
}

class func post(url: String?, param: Parameters, complete: @escaping (HTTPURLResponse?, Result<Any>) -> Void ) -> Void {
        
  // 如果发过来的链接长度为空，则直接返回，不做任何反应
  guard let urlString = url, strlen(url) > 0 else {
    return
  }
        
  Alamofire.request(urlString, method: .post, parameters: param).responseJSON { (response) in
    complete(response.response, response.result)
  }
}
```


> swift 中一个类可以嵌套定义另外类，新增加的类只能被当前类使用
> 
> 在 swift 中，要解除闭包的 循环引用，可以在闭包定义中使用 [unowned self] 或者 [weak self]，其中：
> 
> [unowned self] 类似与 OC 中的 unsafe_unretained，如果对象被释放，仍然保留一个 无效引用，不是可选项
> 
> [weak self] 类似与 OC 中的 __weak，如果对象被释放，自动指向 nil，更安全
> 
> [weak self] 时时监控性能较差，[unowned self]可能导致野指针错误，如果能够确定对象不会被释放，尽量使用 unowned


**懒加载的闭包**

```swift
lazy var printName: ()->() = { [unowned self] in
  print(self.name)
}
```

 

**下面这句不会发生发生循环引用因为执行完就会消失**

```swift
lazy var title: String = {

  print(self.name)
  return "Mr. \(self.name)"
}()
```