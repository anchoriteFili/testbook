---
layout: post
#asp中cookie的添加 
title:  asp中cookie的添加 
#时间配置
date:   2016-08-01 12:18:50 +0800
#大类配置
categories: 知识
#小类配置
tag: asp
---

* content
{:toc}

```c#

<%@LANGUAGE="VBSCRIPT" CODEPAGE="65001"%>
<%
dim numvisits
response.Cookies("NumVisits").Expires = date+365
numvisits = request.Cookies("NumVisits")

if numvisits = "" then
    response.Cookies("NumVisits") = 1
    response.Write("欢迎，这是你第一次进入网页")
else
    response.Cookies("NumVisits") = numvisits + 1
    response.Write("你已经浏览这个页面")
    response.Write("页面" & numvisits)
    if numvisits = 1 then
        response.Write("time before!")
    else
        response.Write("times before!")
    end if
end if
%>

<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Cookie创建</title>
</head>

<body>
</body>
</html>
```