---
layout: post
#标题
title:  Python元组(tuple)
#时间配置
date:   2016-05-25 15:58:22 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}


**元组中只有一个元素时，需要在元素后面添加逗号**

```py
tup1 = (50,) # 元组中只有一个元素时，需要在元素后面添加逗号
```

**元组中的元素值是不允许修改的， 但是我们可以对元组进行连接组合**

```py
# 元组中的元素值是不允许修改的，但是我们可以对元组进行连接组合
tup1 = (12, 34.56)
tup2 = ('ads', 'afa')

#tup1[0] = 100 # 这个是非法的

# 可以进行元组之间的连接组合创建新的元组
tup3 = tup1 + tup2
print tup3, '元组连接组合可以创建新的元组'
```

**元组中的元素值是不允许删除的，但我们可以使用del语句来删除整个元组**

```py
# 元组中的元素值是不允许删除的，但我们可以使用del语句来删除整个元组
tup = ('afafa', 'rqweqwe', 'jljlkjkljkl')
print tup
del tup # 删除元组
```

**元组运算符**

```py
tup = (123, 456, 'afadf')
tup4 = (12, 23, 45)

# 元组运算符 len() 元素个数 + 元组组合 * 元组重复合并成一个元组 in 包含 遍历
print len(tup), '获取元组元素个数'
print tup + tup4, '元组合并'
print tup * 4, '4个重复元组组合成一个元组'
print 123 in tup, '123是否在tup内'
for x in tup: print x # 遍历元组中的元素
```

**无关闭合符 任意无符号的对象，以逗号隔开，默认为元组**

```py
# 无关闭分隔符 任意无符号的对象，以逗号隔开，默认为元组
print 'abc', -4.24e93, 18+6.6j, 'xyz'
x, y = 1, 2
print x, y
```

> 元组内置函数 cmp(tuple1, tuple2)比较两个元组元素 len(tuple) 计算元组中元素个数 max(tuple) 返回元组中元素的最大值 min(tuple) 返回元组中元素最小值。 tuple(seq) 将列表转换为元组