---
layout: post
#标题
title:  swift do-try-catch错误处理模式
#时间配置
date:   2016-05-12 10:19:22 +0800
#大类配置
categories: 知识
#小类配置
tag: swift
---

* content
{:toc}


**<a href="https://www.douban.com/note/531440065/" target="_blank">完整的do-try-catch错误处理</a>模式语法如下：**

```swift
do {

　　try语句

　　成功处理语句组

} catch 匹配错误 {

　　错误处理语句组

}
```

```swift
/**
         * 使用do-try-catch错误模式执行方法
         */
        do {
            
            let script = try String(contentsOfFile: sourcePath!, encoding: NSUTF8StringEncoding)
            JPEngine.evaluateScript(script)
        } catch let err as NSError {
            err.description
        }
```

> 在try语句中可以产生错误，当然也可能不会产生错误，如果有错误发生，catch就会处理错误。
>
> catch代码块可以有多个，错误由哪个catch代码块处理是由catch后面的错误疲惫与否而定的。
>
> 错误类型的多少就决定了catch可以有多少