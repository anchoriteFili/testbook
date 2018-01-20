---
layout: post
#cocoapods安装
title:  cocoapods安装
#时间配置
date:   2015-06-15 14:59:22 +0800
#大类配置
categories: 环境配置
#小类配置
tag: cocoapods
---

* content
{:toc}


**更新安装ruby :** <a href="http://www.07net01.com/2015/09/933234.html" target="_blank">如何在Mac 终端升级ruby版本</a><br>


```shell
$ curl -L get.rvm.io | bash -s stable
$ source ~/.rvm/scripts/rvm
$ rvm -v   // 如果显示版本号，表示添加完成
$ rvm list known // 列出ruby可安装版本
$ rvm install 2.1.4 // 安装最新版本
```
**终端输入ruby -v 看看有没有这个环境**

![151424082324938.png]({{ site.img_url }}67D38AB7B292CDEF0C4FD15F5CAD8A3E.png)

> 输入`gem sources` 出现`https://rubygems.org/`，改变下载源
>
> 输入`gem sources --remove https://rubygems.org/` 移除掉本来的网址
>
> 输入`gem sources -a https://ruby.taobao.org/` 添加指向淘宝的网址，改变下载源

![151436431702435.png]({{ site.img_url }}CDFAE82BF4B57AAB079F05E5243D6995.png)

> 输入`sudo gem install cocoapods` 提示输入`电脑密码`

![151502031543503.png]({{ site.img_url }}240363DA9278CA0A589ADAE32295EF71.png)

**等待-----开始跑**

![151458312957961.png]({{ site.img_url }}20ABA96B0156D4A24315349B6A3DAC21.png)

> 输入pod setup命令，刷新pod库，等待---

**得到所有的第三方的信息**

![151520504517907.png]({{ site.img_url }}F6EE1A55C069F231319956216AF2AF2A.png)

**终端输入cd 拖进去工程文件夹，形成路径**

![151524000915537.png]({{ site.img_url }}6D33547458D1DE60ACFF7A66264A208E.png)

**输入touch Podfile，在文件夹下添加Podfile文件**

![151526222792033.png]({{ site.img_url }}0F405E02229B31B5591EE85CFEF5248F.png)

**输入ls可以查看文件夹下所有文件**

![151528307011166.png]({{ site.img_url }}49EE1394141BC330CF96F02CF6E41A1F.png)

**直接打开Podfile，里边输入第三方的名字pod**

**"KRVideoPlayer"，双引号容易出错**

**新版本必须添加target才能够使用**

![752372-20161019175852404-911238696.png]({{ site.img_url }}DF60563116CD7FEA9732D3067B920317.png)

```shell
target '压缩解压文件' do
end
pod 'SSZipArchive', '~> 1.5'

```

**在终端直接输入pod install 等待-----**

![151540150764142.png]({{ site.img_url }}C7E3B681C542AB7360ED0A729DFA6CFD.png)

**下载完成**

![151545224043484.png]({{ site.img_url }}4E5B5181345F695B24DB684BFBA93771.png)

**第三方添加到了Pods文件夹中**

![151547226703510.png]({{ site.img_url }}720FCFA9326193D9DBAFB12965C68A3B.png)

**添加Pods后，会形成新的启动文件**

 ![151627090133434.png]({{ site.img_url }}6E802FE056C3A94CE207B27C3F0F4EA2.png)

**用这个启动文件，才能显示Pods所有内容**

**添加Alcatraz(插件管理器)命令**

<a href="http://blog.csdn.net/xuping901022xp/article/details/51730778" target="_blank">Mac系统升级Git</a><br>
<a href="http://www.cnblogs.com/wendingding/p/4964661.html" target="_blank">Alcatraz的安装和使用</a><br>





```shell
curl -fsSL https://raw.github.com/alcatraz/Alcatraz/master/Scripts/install.sh | sh
```
**添加第三方控件**

![151630401235306.png]({{ site.img_url }}DD18CB8C47C1ADE851CD70D394242437.png)

**可以实现所有的功能了**

![151633551709718.png]({{ site.img_url }}330292B991DABC5490E99526EC75075C.png)

**如果出现这个错误**

![752372-20151208113155886-1735052568.jpg]({{ site.img_url }}D44B66EF51BD881AC7ED3798E7FDA127.jpg)

**则在终端中输入dirname `which pod`**

![752372-20151208113440449-275179601.png]({{ site.img_url }}4D008B87C56F9ED815F257793D32B755.png)

**将GEM_PATH中的路径改成搜出来的路径即可**

![752372-20151208113607590-365329957.png]({{ site.img_url }}1792906C1AB9BDFABE8D0CE437A39D51.png)

**cocopods版本检查更新命令**

**检查版本命令：`$ pod --version`**

**更新版本：`$ sudo gem install cocoapods --pre`**