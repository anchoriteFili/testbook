---
layout: post
#.gitignore的使用
title:  .gitignore的使用
#时间配置
date:   2018-01-05 15:46:34 +0800
#大类配置
categories: 知识
#小类配置
tag: git
---

* content
{:toc}


#### 引用链接
---

<a href="http://blog.csdn.net/gjy211/article/details/51607347" target="_blank">Github使用.gitignore文件忽略不必要上传的文件</a><br>

**注意**
> 这个文件的完整文件名就是“.gitignore”，注意最前面有个“.”


#### 常用规则
---

> /mtk/ 过滤整个文件夹<br>
> *.zip 过滤所有.zip文件<br>
> /mtk/do.c 过滤某个具体文件<br>

**我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理。那么我们就需要使用：**

> /mtk/<br>
> !/mtk/one.txt

#### 配置语法：

> 以斜杠“/”开头表示目录；<br>
> 以星号“*”通配多个字符；<br>
> 以问号“?”通配单个字符<br>
> 以方括号“[]”包含单个字符的匹配列表；<br>
> 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

> 此外，git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效；

##### 示例

> 规则：fd1/*<br>
> 说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

> 规则：/fd1/*<br>
> 说明：忽略根目录下的 /fd1/ 目录的全部内容；

> 规则：/*<br>
!.gitignore<br>
!/fw/bin/<br>
!/fw/sf/<br>

> 说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；

> 忽略所有 .a 结尾的文件<br>
> *.a<br>
> 但 lib.a 除外<br>
> !lib.a<br>
> 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO<br>
> /TODO<br>
> 忽略 build/ 目录下的所有文件<br>
> build/<br>
> 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt<br>
> doc/*.txt<br>