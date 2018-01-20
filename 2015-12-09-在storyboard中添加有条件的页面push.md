---
layout: post
#标题
title: 在storyboard中添加有条件的页面push
#时间配置
date:   2015-12-09 11:41:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

#### 使用segue进行推
---

##### 1. 按照惯例进行页面的添加跳转

![752372-20151209113400246-128614371.png]({{ site.img_url }}6A74C2D33AC5D6A3925498394D9E7192.png)

##### 2. 设置segue

![752372-20151209113529090-944623073.png]({{ site.img_url }}99925060732446711C56EFCF9A03117B.png)

##### 3. 使用代码直接进行跳转

```objc
[self performSegueWithIdentifier:@"vcToMessageVC" sender:self];
```

**使用storyboard id 进行跳转**

```objc
UIStoryboard *storyBoard = [UIStoryboard storyboardWithName:@"Storyboard" bundle:nil];
UIViewController *viewController = [storyBoard instantiateViewControllerWithIdentifier:@"test2"]; //test2为viewcontroller2的StoryboardId
     
[self.navigationController pushViewController:viewController animated:YES];
```