---
layout: post
#js中lean_cloud相关数据
title:  js中lean_cloud相关数据
#时间配置
date:   2016-06-01 20:57:22 +0800
#大类配置
categories: 知识
#小类配置
tag: js
---

* content
{:toc}

**js部分**

```js
// 应用 ID，用来识别应用
var APP_ID = 'gzGzoHsz';

// 应用 Key，用来校验权限（Web 端可以配置安全域名来保护数据安全）
var APP_KEY = 'YRGG8';

// 初始化
AV.init({
  appId: APP_ID,
  appKey: APP_KEY
});

var XiaoShuoObject = AV.Object.extend('FilmList');


// 存储数据
function saveObjectWith() {
    
var xiaoShuoObject = new XiaoShuoObject();
xiaoShuoObject.save({
  title: 'abc123',
  url: 'abc123',
  volume: 'abc123'
}).then(function() {
  alert('LeanCloud works!');
}).catch(function(err) {
  alert('error:' + err);
});
    
}

// 获取存储数据
function getObjectContent(index, pageCount) {
    
var query = new AV.Query('FilmList');
query.skip(index * pageCount); //跳过的数据
query.limit(pageCount); //返回的条数
query.find().then(function(results) {
  console.log('Successfully retrieved ' + results.length + ' posts.');
  // 处理返回的结果数据
  var movies = new Array();
  for (var i = 0; i < results.length; i++) {
    var object = results[i];
    console.log(object.id + ' - ' + object.get('MovieImageUrl') + object.get('MovieName') + object.get('MovieUrl'));
    var movie = {
        MovieImageUrl: object.get('MovieImageUrl'),
        MovieName: object.get('MovieName'),
        MovieUrl: object.get('MovieUrl')
        
    }
    imageUrl = 'image' + i;
    imageLinkUrl = 'imagea' + i;
    document.getElementById(imageUrl).src = movie.MovieImageUrl;
    document.getElementById(imageUrl).alt = movie.MovieName;
    document.getElementById(imageLinkUrl).href = movie.MovieUrl;
    // movies[i] = movie;
  }
  
  // return movies;
}, function(error) {
  console.log('Error: ' + error.code + ' ' + error.message);
});

}
```

**HTML部分**

```html
<!doctype html>
<html>
<head>
<meta charset="UTF-8">
<title>轮播图自己测试版</title>
<script src="https://cdn1.lncld.net/static/js/av-min-1.0.0.js"></script>
<script src="leanCloud.js"></script>


</head>

<body> 
<button type="button" onClick="saveObjectWith()">保存</button>
<button type="button" onClick="getObjectContent(1, 16)">第一页</button>
<button type="button" onClick="getObjectContent(2, 16)">第二页</button>
<button type="button" onClick="getObjectContent(3, 16)">第三页</button>
<button type="button" onClick="getObjectContent(4, 16)">第四页</button>
<button type="button" onClick="getObjectContent(5, 16)">第五页</button>
<button type="button" onClick="getObjectContent(6, 16)">第六页</button>
<button type="button" onClick="getObjectContent(7, 16)">第七页</button>
<button type="button" onClick="getObjectContent(8, 16)">第八页</button>
<button type="button" onClick="getObjectContent(9, 16)">第九页</button>


<br>
<a target="_blank" id="imagea0"><img id="image0" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea1"><img id="image1" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea2"><img id="image2" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea3"><img id="image3" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea4"><img id="image4" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea4"><img id="image4" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea5"><img id="image5" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea6"><img id="image6" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea7"><img id="image7" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea8"><img id="image8" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea9"><img id="image9" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea10"><img id="image10" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea11"><img id="image11" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea12"><img id="image12" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea13"><img id="image13" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea14"><img id="image14" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea15"><img id="image15" border="0" width="304" height="228"></a>
<a target="_blank" id="imagea16"><img id="image16" border="0" width="304" height="228"></a>




</body> 
</html>
```