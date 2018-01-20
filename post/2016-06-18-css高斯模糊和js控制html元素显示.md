---
layout: post
#css高斯模糊和js控制html元素显示
title:  css高斯模糊和js控制html元素显示
#时间配置
date:   2016-06-18 00:20:22 +0800
#大类配置
categories: 知识
#小类配置
tag: css
---

* content
{:toc}

```css
.blur {    
    filter: url(blur.svg#blur); /* FireFox, Chrome, Opera */
    
    -webkit-filter: blur(10px); /* Chrome, Opera */
       -moz-filter: blur(10px);
        -ms-filter: blur(10px);    
            filter: blur(10px);
    
    filter: progid:DXImageTransform.Microsoft.Blur(PixelRadius=10, MakeShadow=false); /* IE6~IE9 */
}
```

```js
document.getElementById("EleId").style.visibility="hidden";
document.getElementById("EleId").style.visibility="visible";

document.getElementById("EleId").style.display="none";
document.getElementById("EleId").style.display="inline";
```