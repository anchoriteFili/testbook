---
layout: post
#h5添加背景音乐
title:  h5添加背景音乐
#时间配置
date:   2017-12-29 18:35:16 +0800
#大类配置
categories: 示例代码
#小类配置
tag: html
---

* content
{:toc}

### app中环境配置 
---
[ios WKWebView 和UIWebView 播放没有声音的方案](http://blog.csdn.net/yinyakun/article/details/53992474)

**WKWebView设置**
```objc
WKWebViewConfiguration *config = [[WKWebViewConfiguration alloc] init];  
    config.allowsInlineMediaPlayback = YES;  
    config.mediaPlaybackRequiresUserAction = false;  
      
    displayWebView=[[WKWebView alloc] initWithFrame:rect configuration:config];  
    displayWebView.UIDelegate=self;  
    displayWebView.navigationDelegate=self;  

```

**UIWebView设置**

```objc
[self.webView setMediaPlaybackRequiresUserAction:NO];
````

[H5案例分享：解决H5中背景音乐无法自动播放问题](https://www.h5-share.com/articles/201701/bgmusicarticle.html)<br>


<!-- 音乐 start-->
<div class="musicinfo" id="musicinfo">
    <audio id="musicid" src="/styles/audio/message.mp3" preload="preload" autoplay="autoplay"  loop="loop">您的浏览器不支持 audio标签。</audio>
</div>
<!-- 音乐 end-->
<script type="text/javascript">
	// 音乐播放
	function autoPlayMusic() {
	    // 自动播放音乐效果，解决浏览器或者APP自动播放问题
	    function musicInBrowserHandler() {
	        musicPlay(true);
	        document.body.removeEventListener('touchstart', musicInBrowserHandler);
	    }
	    document.body.addEventListener('touchstart', musicInBrowserHandler);

	    // 自动播放音乐效果，解决微信自动播放问题
	    function musicInWeixinHandler() {
	        musicPlay(true);
	        document.addEventListener("WeixinJSBridgeReady", function () {
	            musicPlay(true);
	        }, false);
	        document.removeEventListener('DOMContentLoaded', musicInWeixinHandler);
	    }
	    document.addEventListener('DOMContentLoaded', musicInWeixinHandler);
	}
	function musicPlay(isPlay) {
	    var audio = document.getElementById('musicid');
	    if (isPlay && audio.paused) {
	        audio.play();
	    }
	    if (!isPlay && !audio.paused) {
	        audio.pause();
	    }
	}
	autoPlayMusic();
</script>
