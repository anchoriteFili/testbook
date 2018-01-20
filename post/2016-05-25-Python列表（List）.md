---
layout: post
#标题
title:  Python列表（List）
#时间配置
date:   2016-05-25 15:23:22 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}





**查询列表元素**

```py
list2 = [1, 2, 3, 4, 5, 6, 7]

print list1[2] # 查找元素
print list2[1:5] # 返回包含范围内元素的列表
```

**删除列表元素**

```py
list2 = [1, 2, 3, 4, 5, 6, 7]

del list2[2]
print list2
```
**Python列表脚本操作符**

```py
# Python列表脚本操作符
a = [1, 2, 3]
b = [4, 5, 6]
print a + b # + 用于组合列表
print len(a) # len输出列表元素个数
print a * 4 # * 用于重复列表
print 3 in a # in 3是否是a中的一个元素 返回True
for x in a: print x # 遍历所有元素
```

**Python列表函数&方法**

**Python包含以下函数**

```py
'''
    Python包含以下函数 cmp(list1, list2) len(list) max(list) min(list) list(seq)
'''

# Python列表函数&方法 .cmp() 用于比较两个列表的元素 有什么用？
print cmp(a, b) # 返回 -1
print cmp(b, a) # 返回 1
a += [1, 2, 3]
print cmp(a, b) # 返回 -1

print len(a) # 列表中元素的个数
print max(a) # 返回列表中的最大值
print min(a) # 返回列表中的最小值
seq = (1, 2, 3)
print list(seq) # 将元组转换成列表
```


**Python包含以下方法**

```py
'''
    Python包含以下方法
'''

print a.append('afaf') # 在列表的末尾添加新的对象
print a.count(1), '统计某个元素在列表中出现的次数'

a.extend(seq)
print a, '在列表的末尾一次性追加另一个序列中的多个值'
print a.index(1), '从列表中找出某个值第一个匹配项的索引位置'

a.insert(2, 9)
print a, '将9插入到索引为2的位置，原来的2后移一位'

# .pop()函数用于移除列表中的一个元素（默认最后一个元素），并且返回该元素的值。
aList = [123, 'afaf', 'afadf', 'asfa', 123]
print aList.pop(), '默认移除最后一个元素，返回最后一个元素'
print aList.pop(2), '移除第二个元素，返回移除的第二个元素'

# .remove(obj) 移除列表中某个值的第一个匹配项 没有返回值
print aList
aList.remove(123) # 写成中括号，报错 'builtin_function_or_method' object has no attribute '__getitem__'
print aList, '移除与123匹配的第一个匹配项'


# reverse()函数用于反向列表中元素
aList.reverse()
print aList, '反向列表中的元素'

# sort()函数用于对原列表进行排序，如果指定参数，则使用比较函数指定的比较函数
bList = [123, 'xyz', 'zara', 'abc']
bList.sort()
print bList, '对原列表进行排序'
```