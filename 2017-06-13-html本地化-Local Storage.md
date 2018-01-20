---
layout: post
#html本地化-Local Storage
title:  html本地化-Local Storage
#时间配置
date:   2017-06-13 17:26:16 +0800
#大类配置
categories: 知识
#小类配置
tag: html
---

* content
{:toc}

```js
// 永久存储(示例)
// 存储
localStorage.lastname = "Gates";
// 取回
document.getElementById("result").innerHTML = localStorage.lastname;
// 移除
localStorage.removeItem("lastname");

// 实例
if (localStorage.clickcount) {
    localStorage.clickcount = Number(localStorage.clickcount) + 1;
} else {
    localStorage.clickcount = 1;
}
document.getElementById("result").innerHTML = "您已经点击这个按钮 " +
localStorage.clickcount + " 次。";


// 暂时存储，删除页面即消失（示例）
if (sessionStorage.clickcount) {
    sessionStorage.clickcount = Number(sessionStorage.clickcount) + 1;
} else {
    sessionStorage.clickcount = 1;
}
document.getElementById("result").innerHTML = "在本 session 中，您已经点击这个按钮 " +
sessionStorage.clickcount + " 次。";
```