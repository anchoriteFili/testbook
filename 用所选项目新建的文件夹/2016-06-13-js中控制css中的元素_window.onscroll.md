---
layout: post
#js中控制css中的元素_window.onscroll
title:  js中控制css中的元素_window.onscroll
#时间配置
date:   2016-06-13 11:53:22 +0800
#大类配置
categories: 知识
#小类配置
tag: js
---

* content
{:toc}

```html
<!doctype html>
<html>
<head>
<meta charset="UTF-8">
<title>公会驻地</title>
<link rel="stylesheet" type="text/css" href="css练习.css">
<script src="css练习.js"></script>

<style type="text/css"> 
#top_div{ 
    top:80px; 
    right:0; 
    display:inline; 
} 
</style> 
<script type="text/javascript"> 
window.onscroll = function(){ 
    var t = document.documentElement.scrollTop || document.body.scrollTop;  
    var top_div = document.getElementById( "top_div" ); 
    if( t >= 300 ) { 
        top_div.style.position = "fixed";
    } else { 
        top_div.style.position = "static";
    } 
} 

</script>

</head>

<a name="top">顶部<a> 
<div id="top_div"><a href="#top">返回顶部</a></div> 
<br>
<br>
<div> 
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
这里尽量多些<br />
 
</div>
</body>
</html>
```