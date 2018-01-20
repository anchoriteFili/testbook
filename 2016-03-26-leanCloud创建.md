---
layout: post
#标题
title: leanCloud创建
#时间配置
date:   2016-03-26 10:02:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}


**AVObject 中字段允许的类型包括：**

* String 字符串
* Number 数字
* Boolean 布尔类型
* Array 数组
* Object 对象
* Date 日期
* Bytes base64编码的二进制数据
* File 文件
* Null 空值

> Object 类型简单地表示每个字段的值都可以由能 JSON 编码的内嵌对象组合而成。对象的 Key（键）包含$或者.，或者同时有 __type 的键，都是系统保留用来做一些额外处理的特殊键，因此请不要在你的对象中使用这样的 Key。另外，code、uuid、className、keyValues、fetchWhenSave、running、acl、ACL、isDataReady、pendingKeys、createdAt、updatedAt、objectId、description 也都是保留字段，不能作为键来使用。

 
#### <a href="https://leancloud.cn/docs/storage_overview.html#数据管理" target="_blank">数据存储服务总览</a><br>
---





> <a href="https://leancloud.cn/data.html?appid={{appid}}" target="_blank">数据管理平台</a>是一个允许在任何 app 里更新或者创建对象的 Web UI 管理平台。在这里，你可以看到保存在 Class 里的每个对象的原生 JSON 值。当使用这个平台的时候，请牢记： 

 

* 输入 "null" 将会设置值为特殊的空值 null，而非字符串 "null"。
* objectId, createdAt 和 updatedAt 不可编辑（它们都是系统自动设置的）。
* 下划线开始的 class 为系统内置 class，不可删除，并且请轻易不要修改它的默认字段，可添加字段。