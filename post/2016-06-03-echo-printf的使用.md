---
layout: post
#echo-printf的使用
title:  echo-printf的使用
#时间配置
date:   2016-06-03 23:04:22 +0800
#大类配置
categories: 知识
#小类配置
tag: shell
---

* content
{:toc}

**echo使用**

```shell
#!/bin/bash


# Shell echo命令
# Shell 的 echo命令与PHP的echo指令类似，都是用于字符串的输出
echo string
# 这里双引号可以省略

# 1. 显示普通字符串
echo "It is a test"

echo It is a test

# 2. 显示转移字符
echo "\"It is a test\""

# 3. 显示变量
# read命令从标准输入中读取一行，并把输入行的每个字段的值指定给shell变量
# read name
# echo "$name It is a test"

# 4. 显示换行
echo -e "OK! \n" # -e开启转译
echo -e "OK!"
echo "OK! \n"

# 5. 显示不换行
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"

# 6. 显示结果定向至文件
# echo "It is test" > myfile

# 7. 用单引号原样输出字符串，不进行转义或取变量
echo '$name\"'

# 8. 显示命令执行结果
echo `date`
```

**printf的使用**

```shell
# printf 使用引用文本或空格分隔的参数，外面可以在printf中使用格式化字符串，还可以指定字符串的
# 宽度、左右对齐方式等。默认printf不会像echo自动添加换行符，我们可以手动添加\n

# printf format-string [arguments...]
# format-string: 为格式控制字符串
# arguments: 为参数列表

# %s %c %d %f 都是格式替换符
# %-10s 指一个宽度为10个字符（-表示左对齐，没有表示右对齐），任何字符都被显示在10个字符宽度内
# 如果不足则自动一空格填充，超过也会将内容全部显示出来
# %4.2f指格式化为小数，其中.2指保留2位小数
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
printf "%-10s %-8s %-4.2f\n" 杨过 男 38.233223
printf "%-10s %-8s %-4.2f\n" 芙蓉 女 12.12121

# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样
printf '%d %s\n' 1 "abc"

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```