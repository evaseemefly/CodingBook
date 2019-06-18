本笔记为 python 核心技术与实战的笔记  
python 的知识点图谱
![知识图谱](\img\01\1.png)

## 二 基础篇

### 列表和元组

几个内置函数  
`count(item)` 表示统计列表 / 元组中 item 出现的次数。  
`index(item)` 表示返回列表 / 元组中 item 第一次出现的索引  
`list.reverse()`和 `list.sort()` 倒叙和排序（列表才有）  
`reversed()` 和 `sorted()` 同样表示对列表 / 元组进行倒转和排序，但是会返回一个倒转后或者排好序的新的列表 / 元组。

#### 列表和元组的性能

元组要比列表更轻量级一些，元组的速度要`略优于列表`

#### 列表和元组的使用场景

1. 如果存储的数据和数量不变，比如你有一个函数，需要返回的是一个地点的经纬度，然后直接传给前端渲染，那么肯定选用元组更合适。

```python
def get_location():
    .....
    return (longitude, latitude)

```

2. 如果存储的数据或数量是可变的，比如社交平台上的一个日志功能，是统计一个用户在一周之内看了哪些用户的帖子，那么则用列表更合适。

```python
viewer_owner_id_list = [] # 里面的每个元素记录了这个 viewer 一周内看过的所有 owner 的 id
records = queryDB(viewer_id) # 索引数据库，拿到某个 viewer 一周内的日志
for record in records:
    viewer_owner_id_list.append(record.id)

```

总结：
总的来说，列表和元组都是有序的，可以存储任意数据类型的集合，区别主要在于下面这两点。  
列表是动态的，长度可变，可以随意的增加、删减或改变元素。列表的存储空间略大于元组，性能略逊于元组。  
元组是静态的，长度大小固定，不可以对元素进行增加、删减或者改变操作。元组相对于列表更加轻量级，性能稍优。

思考：

1. 想创建一个空的列表，我们可以用下面的 A、B 两种方式，请问它们在效率上有什么区别吗？我们应该优先考虑使用哪种呢？可以说说你的理由。

```python
# 创建空列表
# option A
empty_list = list()

# option B
empty_list = []

```

[]比 list()更快，因为调用 list 函数有一定的开销，而[]却没有。

1. 区别主要在于 list()是一个 function call，Python 的 function call 会创建 stack，并且进行一系列参数检查的操作，比较 expensive，反观[]是一个内置的 C 函数，可以直接被调用，因此效率高

S:  
测试时间的方式：

```python
python -m timeit 'empty_list = list()'
# 10000000 loops, best of 3: 0.0829 usec per loop

python -m timeit 'empty_list = []'
# 10000000 loops, best of 3: 0.0218 usec per loop

python -m timeit 'empty_list = ()'
# 100000000 loops, best of 3: 0.0126 usec per loop
```

### 排序

sort 函数：

```python
sorted(iterable[, key][, reverse])
list.sort(*, key=None, reverse=None)
```

- key 是带一个参数的函数，返回一个值用来排序，默认为 None。这个函数只调用一次，所以 fast。
- reverse 表示排序结果是否反转

eg:

```python
>>> a = (1,2,4,2,3)   # a 是元组，故不能用sort() 排序
>>> a.sort()
Traceback (most recent call last):
 File "<stdin>", line 1, in <module>
AttributeError: 'tuple' object has no attribute 'sort'
>>> sorted(a)    # sorted() 可以为元组排序，返回一个新有序列表
[1, 2, 2, 3, 4]

>>> a=['1',1,'a',3,7,'n']
>>> sorted(a)
[1, 3, 7, '1', 'a', 'n']
>>> a     # sorted() 不改变原列表
['1', 1, 'a', 3, 7, 'n']
>>> print a.sort()
None
>>> a      # a.sort()直接修改原列表
[1, 3, 7, '1', 'a', 'n']
```

因此如果实际应用过程中需要保留原有列表，使用 sorted() 函数较为适合，否则可以选 择 sort() 函数，因为 sort() 函数不需要复制原有列表，消耗的内存较少，效率也较高。

### 输入与输出

eg：

1. 读取文件；
2. 去除所有标点符号和换行符，并把所有大写变成小
3. 合并相同的词，统计每个词出现的频率，并按照词频从大到小排序；
4. 将结果按行输出到文件 out.txt。

```python
import re

# 你不用太关心这个函数
def parse(text):
    # 使用正则表达式去除标点符号和换行符
    text = re.sub(r'[^\w ]', ' ', text)

    # 转为小写
    text = text.lower()

    # 生成所有单词的列表
    word_list = text.split(' ')

    # 去除空白单词
    word_list = filter(None, word_list)

    # 生成单词和词频的字典
    word_cnt = {}
    for word in word_list:
        if word not in word_cnt:
            word_cnt[word] = 0
        word_cnt[word] += 1

    # 按照词频排序
    # 注意此处的key=lambda kv:kv[1]，找到频率
    sorted_word_cnt = sorted(word_cnt.items(), key=lambda kv: kv[1], reverse=True)

    return sorted_word_cnt

with open('in.txt', 'r') as fin:
    text = fin.read()

word_and_freq = parse(text)

with open('out.txt', 'w') as fout:
    for word, freq in word_and_freq:
        fout.write('{} {}\n'.format(word, freq))

########## 输出 (省略较长的中间结果) ##########

and 15
be 13
will 11
to 11
the 10
of 10
a 8
we 8
day 6

...

old 1
negro 1
spiritual 1
thank 1
god 1
almighty 1
are 1

```

### 条件与循环

字典的遍历  
可以通过 `.values()`与 `.items()`分别遍历字典的 value 与 键值对

```python
d = {'name': 'jason', 'dob': '2000-01-01', 'gender': 'male'}
for k in d: # 遍历字典的键
    print(k)
name
dob
gender

for v in d.values(): # 遍历字典的值
    print(v)
jason
2000-01-01
male

for k, v in d.items(): # 遍历字典的键值对
    print('key: {}, value: {}'.format(k, v))
key: name, value: jason
key: dob, value: 2000-01-01
key: gender, value: male

```

遍历集合，不仅返回每个元素，并且还返回其对应的索引

```python
l = [1, 2, 3, 4, 5, 6, 7]
for index, item in enumerate(l):
    if index < 5:
        print(item)

1
2
3
4
5

```

在循环语句中，我们还常常搭配 continue 和 break 一起使用。所谓 continue，就是让程序跳过`当前这层循环，继续执行下面的循环`；而 break 则是指`完全跳出所在的整个循环体`。在循环中适当加入 continue 和 break，往往能使程序更加简洁、易读。  
eg:

- 比如，给定两个字典，分别是产品名称到价格的映射，和产品名称到颜色列表的映射。我们要找出价格小于 1000，并且颜色不是红色的所有产品名称和颜色的组合。如果不用 continue，代码应该是下面这样的：

```python
# name_price: 产品名称 (str) 到价格 (int) 的映射字典
# name_color: 产品名字 (str) 到颜色 (list of str) 的映射字典
for name, price in name_price.items():
    if price < 1000:
        if name in name_color:
            for color in name_color[name]:
                if color != 'red':
                    print('name: {}, color: {}'.format(name, color))
        else:
            print('name: {}, color: {}'.format(name, 'None'))

```

使用 continue

```python
# name_price: 产品名称 (str) 到价格 (int) 的映射字典
# name_color: 产品名字 (str) 到颜色 (list of str) 的映射字典
for name, price in name_price.items():
    if price >= 1000:
        continue
    if name not in name_color:
        print('name: {}, color: {}'.format(name, 'None'))
        continue
    for color in name_color[name]:
        if color == 'red':
            continue
        print('name: {}, color: {}'.format(name, color))

```

前面讲了 for 循环，对于 while 循环，原理也是一样的。它表示当 condition 满足时，一直重复循环内部的操作，直到 condition 不再满足，就跳出循环体。

```python
while condition:
    ....

```

列表推导

```python
expression1 if condition else expression2 for item in iterable

```

等价于

```python
for item in iterable:
    if condition:
        expression1
    else:
        expression2

```

对于没有 else 的

```python
expression for item in iterable if condition

```

思考题：  
最后给你留一个思考题。给定下面两个列表 attributes 和 values，要求针对 values 中每一组子列表 value，输出其和 attributes 中的键对应后的字典，最后返回字典组成的列表。

```python
attributes = ['name', 'dob', 'gender']
values = [['jason', '2000-01-01', 'male'],
['mike', '1999-01-01', 'male'],
['nancy', '2001-02-01', 'female']
]

# expected outout:
[{'name': 'jason', 'dob': '2000-01-01', 'gender': 'male'},
{'name': 'mike', 'dob': '1999-01-01', 'gender': 'male'},
{'name': 'nancy', 'dob': '2001-02-01', 'gender': 'female'}]

```

answer:

```python
[dict(zip(attributes,v)) for v in values]
```
