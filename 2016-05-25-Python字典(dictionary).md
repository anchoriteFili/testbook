---
layout: post
#标题
title:  Python字典(dictionary)
#时间配置
date:   2016-05-25 17:39:22 +0800
#大类配置
categories: 知识
#小类配置
tag: python
---

* content
{:toc}


##### 1. 字典中键必须是唯一的，但值则不必，值可以取任何数据类型，但键必须是不可变的。

##### 2. 访问字典里的值dic[7],如果没有对应的值7，返回错误keyError: 7

##### 3. 修改字典

```py
# 修改字典
# 向字典添加新内容的方法是增加新的键/值对，修改或删除已有键/值对方法如下
dic1[1] = 12020
print dic1, '修改已有元素'
dic1['school'] = 'school'
print dic1, '没有相应的key值，添加新元素'
```

##### 4. 删除字典元素

```py
# 删除字典元素
del dic1[1] # 删除单个元素
print dic1
dic1.clear() # 清空所有条目
print dic1
del dic1 # 删除字典
```

##### 5. 字典键的特性


> 字典键的特性
> 1. 不允许同一个键出现两次，创建时如果同一个键赋值两次，后一个值会被记住
> 2. 键必须不可变，所以可以用数字、字符串、、或元组充当，不能用列表

##### 6. 字典内置函数&方法

###### <1> 字典内置函数

```py
# cmp()函数用于比较两个字典元素
# 返回值 如果两个字典的元素相同返回0， 如果字典dict1大于字典dict2返回1，如果字典dict1小于字典dict2返回-1
dict1 = {'Name': 'Zara', 'Age': 7};
dict2 = {'Name': 'Mahnaz', 'Age': 27};
dict3 = {'Name': 'Abid', 'Age': 27};
dict4 = {'Name': 'Zara', 'Age': 7};
print "Return Value : %d" %  cmp (dict1, dict2) # 返回 -1
print "Return Value : %d" %  cmp (dict2, dict3) # 返回 1
print "Return Value : %d" %  cmp (dict1, dict4) # 返回 0

# len(dic)函数，返回dic中元素个数，也就是key的个数
print len(dict1)

# str(dic) 输出字典可打印的字符串表示
print dict1, '字典形式'
print str(dict1), '字符串形式'

# type()函数返回变量的类型
print type(dict1)
print type(str(dict1))
```

###### <2> 字典内置方法

```py
'''
   Python字典内置方法
'''
dic1 = {1: 'a', 2: 'b', 3: 'c', 4: 'd'}
dic1.clear() # 删除字典内所有的元素

dic1 = {1: 'a', 2: 'b', 3: 'c', 4: 'd'}
print dic1.copy(), '返回一个字典的浅复制'

# fromkeys()函数用于创建一个新字典，以序列seq中元素做字典的键，value为字典所有键对应的初始值
seq = ('name', 'age', 'sex')
dict = dict.fromkeys(seq)
print dict, '没有value初始值'
dict = dict.fromkeys(seq, 10)
print dict, '有初始值10'

# get()函数返回指定键的值，如果只不在字典中返回默认值
print dict.get('age'), '获取年龄'
print dict.get('money'), '如果没有对应Key值，返回None'

# has_key()函数用于判断键是否存在于字典中，如果在返回true，如果不在返回false
print dict.has_key('age'), '判断dict中是否存在键age'

# items()函数以列表返回可遍历的(键，值）元组数组。
dict = {'name': 'lucy', 'age': 7}
print dict.items(), '返回键值组成的元组组成的数组'

# keys()函数以列表返回一个字典的所有的键
print dict.keys(), '返回所有的key组成的列表'

# setdefault()函数和get()方法类似，如果键不在字典中，将会添加键并将值设为默认值
dict = {'name': 'lucy', 'age': 4}
print dict.setdefault('age', None), '获取age，如果存在返回age对应的value值'
print dict.setdefault('sex', None), '获取sex，若果不存在将sex添加到字典，并设初始值为None'
print dict

# update(dic)函数吧字典dic中的键/值对更新到对应的dict里。相当于合并字典
dict = {'name': 'lucy', 'age': 4}
dict2 = {'sex': 'female'}
dict.update(dict2)
print dict

# values()以列表返回字典中的所有的值
print dict.values(), '列表形式返回所有的value值'
```