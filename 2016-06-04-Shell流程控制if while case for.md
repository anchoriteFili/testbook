---
layout: post
#Shell流程控制if while case for 
title:  Shell流程控制if while case for 
#时间配置
date:   2016-06-04 01:11:22 +0800
#大类配置
categories: 知识
#小类配置
tag: shell
---

* content
{:toc}

```shell
# 条件判断语句 if-elif-else-fi
a=10
b=20

if [ $a == $b ]
then
    echo "a 等于 b"
elif [ $a -gt $b ]
then
    echo "a 大于 b"
elif [ $a -lt $b ]
then
    echo "a 小于 b"
else
    echo "没有符合的条件"
fi

# if-else 常与test命令结合使用
num1=$[2*3]
num2=$[1+5]

if test $[num1] -eq $[num2]
then
    echo '两个数字相等'
else
    echo '两个数字不相等'
fi

# for循环
# for var in tiem1 item2 ...itemN; do command1; command2... dome;
# 当变量值在列表里，for循环即执行一次所有命令，使用变量名获取列表中的当前取值。
# 命令可为任何有效的shell命令和语句。in列表可以包含替换、字符串和文件名。
# in列表是可选的，如果不用它，for循环使用命令行的位置参数

for loop in 1 2 3 4 5
do
    echo "the value is : $loop"
done

for str in 'This is a string'
do
    echo $str # 顺序输出字符串中的字符
done

# while语句
# while循环用于不断执行一系列命令，也用于从输入文件中读取数据；命令通常为测试条件
int=1

while (( $int<=5 )) # 必须有两个括号，逗逼
do
    echo $int
    let "int++"
done
# 使用了Bash let命令，它用于执行一个或多个表达式，变量计算中不需要加上$来表示变量。
# while循环可用于读取键盘信息。下面的例子中，输入信息被设置为变量FILM,按<Ctrl-D>结束循环
echo '按下<CTRL-D>退出'
echo '输入你最喜欢的电影名：'
# while read FILM
# do
# echo "是的！$FILM 是一部好电影"
# done


# 无限循环
# while : do command done
# while true do command done


# until循环
# until循环执行一些令命令直至条件为真时停止。
# until循环与while循环在处理方式上刚好相反
# 一般while循环由于until循环，但在某些时候-也是极少数情况下，until循环更加有用。
# until codition do comman done


# case语句为多选择语句。可以用case语句匹配一个值或一个模式，如果匹配成功，执行向匹配的命令
# case工作方式，去之后必须为单词in，每以模式必须以右括号结束。取值可以为变量或常数。匹配
# 发现取值符合某一模式后，期间所有命令开始执行直至;;
# 取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式响应命令后不再继续其他模式。
# 如果无一匹配模式，使用星号*捕捉该值，在执行后面的命令。

aNum=3
case $aNum in
    1) echo '你选择了1'
    ;;
    2) echo '你选择了2'
    ;;
    3) echo '你选择了3'
    ;;
    4) echo '你选择了4'
    ;;
    *) echo '你没有输入1到4之间的数字'
    ;;
esac

# 跳出循环
# break命令
while :
do
    echo -n "输入1到5之间的数字："
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数值为 $aNum"
        ;;
        *) echo "你输入的数字不是1到5之间的！游戏结束"
            break
        ;;
    esac
done

# continue
while :
do
    echo -n "输入1到5之间的数字："
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数值为 $aNum"
        ;;
        *) echo "你输入的数字不是1到5之间的！游戏结束"
            continue
            echo "本轮游戏结束"
        ;;
    esac
done

# case用反过来的写反来结束 esac
```