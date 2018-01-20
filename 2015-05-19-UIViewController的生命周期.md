---
layout: post
#标题
title:  UIViewController的生命周期
#时间配置
date:   2015-05-19 12:44:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

![191046353547477.png]({{ site.img_url }}5B04D6D7BA3FCCA528C36774CD93C41A.png)

![752372-20160316220755693-685341828.jpg]({{ site.img_url }}F32E35BBC24034D8B935DD1F3D51D773.jpg)



```objc
如果出现错误
[1. lowMemoryWarningOccurs

 2. didReceiveMemoryWarning

 3. propertyViewIsReleasedAndSetToNil

 4. viewDidUnload]

viewNotVisibleOnScreen

viewAskedToAppearOnScreen

1. loadView

2. viewDidLoad

3. viewWillAppear

4. viewDidAppear

如果出现错误

[ 5. lowMemoryWarningOccurs  6. didReceiveMemoryWarning ]

7. viewWillDisappear

8. viewDidDisappear
```