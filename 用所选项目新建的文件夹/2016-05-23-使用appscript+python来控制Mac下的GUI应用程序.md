---
layout: post
#标题
title:  使用appscript+python来控制Mac下的GUI应用程序
#时间配置
date:   2016-05-23 14:19:22 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}

<a href="http://willzh.iteye.com/blog/333051" target="_blank">使用appscript+python来控制Mac下的GUI应用程序</a><br>

> 在Mac下，appscript是一个与应用程序通信交互的强大工具。用Python的appscript模块，可以在不用学习appscript的情况下也能做到与很多应用程序交互的功能。 

> 打开Mac的终端，安装很简单： 
sudo easy_install appscript 

**然后运行python，先来试一个简单有趣的例子: **

```py
>> import osax  
>> sa = osax.OSAX()  
>> sa.say("Hello world", using="Victoria")  
```
 
> 怎么样，你的苹果说话了吧──打破通常用无声"Hello world”作为程序入门的惯例 ：） 

**下面是一个比较实用的例子，调用iTunes播放你喜欢的音乐：**

```py
import appscript
iTunes = appscript.app("iTunes")
browserWindows = iTunes.browser_windows()
browserWindow = browserWindows[0]
playList = browserWindow.view()
track = playList.tracks[2]
print "Now playing the 2nd track:"
print "-"*50
print "Name:", track.name()
print "Artist:", track.artist()
print "Genre:", track.genre()
track.play()
```
 
**保存程序文件play2nd.py，运行情况如下：**

```py
$ python play2nd.py 
Now playing the 2nd track:
--------------------------------------------------
Name: Rainmaker
Artist: Yanni
Genre: New Age
```
 
**对于iTunes，我们还可以编写更多实用的例子：**

```py
import appscript  
iTunes = appscript.app("iTunes")  
browserWindows = iTunes.browser_windows()  
browserWindow = browserWindows[0]  
playList = browserWindow.view()  
for i in range(1,10):  
        track  = playList.tracks[i]  
        print "-"*50  
        try:  
                print "Name:", track.name().encode('utf8')  
                print "Artist:", track.artist()  
                print "Genre:", track.genre()  
        except Exception,e:  
                pass  
```
 
**该程序的作用是，列出iTunes第一个列表中的前10首音乐。**

**另外一个工作上比较实用的功能是，appscript+python还可以与FileMakerPro进行数据库操作。这里有一篇文章可供参考： <a href="http://wiki.python.org/moin/MacPython/FileMakerPro/AppscriptingOverview" target="_blank">http://wiki.python.org/moin/MacPython/FileMakerPro/AppscriptingOverview</a><br>**