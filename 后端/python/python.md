本笔记为 python 核心技术与实战的笔记  
python 的知识点图谱
![知识图谱](\img\01\1.png)

## 二 基础篇

### 列表和元组

对已经存在的元祖，可以重新创建新的元祖并为其添加元素
eg:

```python
tup = (1, 2, 3, 4)
new_tup = tup + (5, ) # 创建新的元组 new_tup，并依次填充原元组的值
new _tup
(1, 2, 3, 4, 5)

l = [1, 2, 3, 4]
l.append(5) # 添加元素 5 到原列表的末尾
l
[1, 2, 3, 4, 5]

```

列表与元组之间的相互转换

```python
list((1, 2, 3))
[1, 2, 3]

tuple([1, 2, 3])
(1, 2, 3)

```

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

### 字典和集合基础

`字典`从 python3.7 开始，字典被确定为有序的（3.6 之前是无序的）

```python
d1 = {'name': 'jason', 'age': 20, 'gender': 'male'}
d2 = dict({'name': 'jason', 'age': 20, 'gender': 'male'})
d3 = dict([('name', 'jason'), ('age', 20), ('gender', 'male')])
d4 = dict(name='jason', age=20, gender='male')
d1 == d2 == d3 ==d4
True

s1 = {1, 2, 3}
s2 = set([1, 2, 3])
s1 == s2
True

```

通过 key 来索引字典可以使用`get(key,def)`

```python
d = {'name': 'jason', 'age': 20}
d.get('name')
'jason'
d.get('location', 'null')
'null'

```

`集合（set）`不支持索引操作，集合本质是一个`哈希表`
判断指定元素是否在字典或集合内，可以使用 `val in dict/set` 的方式
eg:

```python
s = {1, 2, 3}
1 in s
True
10 in s
False

d = {'name': 'jason', 'age': 20}
'name' in d
True
'location' in d
False

```

字典与集合均支持增删改查操作：
_注意 dict 与 set 的增加与删除操作的方法不同_

```python
d = {'name': 'jason', 'age': 20}
d['gender'] = 'male' # 增加元素对'gender': 'male'
d['dob'] = '1999-02-01' # 增加元素对'dob': '1999-02-01'
d
{'name': 'jason', 'age': 20, 'gender': 'male', 'dob': '1999-02-01'}
d['dob'] = '1998-01-01' # 更新键'dob'对应的值
d.pop('dob') # 删除键为'dob'的元素对
'1998-01-01'
d
{'name': 'jason', 'age': 20, 'gender': 'male'}

s = {1, 2, 3}
s.add(4) # 增加元素 4 到集合
s
{1, 2, 3, 4}
s.remove(4) # 从集合中删除元素 4
s
{1, 2, 3}

```

对字典或集合进行排序  
对于字典会根据 key 或 val 进行升序或降序排列：

```python
d = {'b': 1, 'a': 2, 'c': 10}
d_sorted_by_key = sorted(d.items(), key=lambda x: x[0]) # 根据字典键的升序排序
d_sorted_by_value = sorted(d.items(), key=lambda x: x[1]) # 根据字典值的升序排序
d_sorted_by_key
[('a', 2), ('b', 1), ('c', 10)]
d_sorted_by_value
[('b', 1), ('a', 2), ('c', 10)]

```

对于集合的排序

```python
s = {3, 4, 2, 1}
sorted(s) # 对集合的元素进行升序排序
[1, 2, 3, 4]

```

eg：
要找出这些商品有多少种不同的价格
使用列表与 set 的区别如下：

```python
# list version
def find_unique_price_using_list(products):
    unique_price_list = []
    for _, price in products: # A
        if price not in unique_price_list: #B
            unique_price_list.append(price)
    return len(unique_price_list)

products = [
    (143121312, 100),
    (432314553, 30),
    (32421912367, 150),
    (937153201, 30)
]
print('number of unique price is: {}'.format(find_unique_price_using_list(products)))

# 输出
number of unique price is: 3

```

```python
# set version
def find_unique_price_using_set(products):
    unique_price_set = set()
    for _, price in products:
        unique_price_set.add(price)
    return len(unique_price_set)

products = [
    (143121312, 100),
    (432314553, 30),
    (32421912367, 150),
    (937153201, 30)
]
print('number of unique price is: {}'.format(find_unique_price_using_set(products)))

# 输出
number of unique price is: 3

```

两者在时间复杂度上差异明显：

```python
import time
id = [x for x in range(0, 100000)]
price = [x for x in range(200000, 300000)]
products = list(zip(id, price))

# 计算列表版本的时间
start_using_list = time.perf_counter()
find_unique_price_using_list(products)
end_using_list = time.perf_counter()
print("time elapse using list: {}".format(end_using_list - start_using_list))
## 输出
time elapse using list: 41.61519479751587

# 计算集合版本的时间
start_using_set = time.perf_counter()
find_unique_price_using_set(products)
end_using_set = time.perf_counter()
print("time elapse using set: {}".format(end_using_set - start_using_set))
# 输出
time elapse using set: 0.008238077163696289

```

_Q_:  
字典的 k 可以使一个列表吗？  
eg：

```python
d = {'name': 'jason', ['education']: ['Tsinghua University', 'Stanford University']}

```

_A_:
不可以，字典的 key 要求是不可变的，而 list 是可变类型

### 字典和集合基础

`字典`从 python3.7 开始，字典被确定为有序的（3.6 之前是无序的）

```python
d1 = {'name': 'jason', 'age': 20, 'gender': 'male'}
d2 = dict({'name': 'jason', 'age': 20, 'gender': 'male'})
d3 = dict([('name', 'jason'), ('age', 20), ('gender', 'male')])
d4 = dict(name='jason', age=20, gender='male')
d1 == d2 == d3 ==d4
True

s1 = {1, 2, 3}
s2 = set([1, 2, 3])
s1 == s2
True

```

通过 key 来索引字典可以使用`get(key,def)`

```python
d = {'name': 'jason', 'age': 20}
d.get('name')
'jason'
d.get('location', 'null')
'null'

```

`集合（set）`不支持索引操作，集合本质是一个`哈希表`
判断指定元素是否在字典或集合内，可以使用 `val in dict/set` 的方式
eg:

```python
s = {1, 2, 3}
1 in s
True
10 in s
False

d = {'name': 'jason', 'age': 20}
'name' in d
True
'location' in d
False

```

字典与集合均支持增删改查操作：
_注意 dict 与 set 的增加与删除操作的方法不同_

```python
d = {'name': 'jason', 'age': 20}
d['gender'] = 'male' # 增加元素对'gender': 'male'
d['dob'] = '1999-02-01' # 增加元素对'dob': '1999-02-01'
d
{'name': 'jason', 'age': 20, 'gender': 'male', 'dob': '1999-02-01'}
d['dob'] = '1998-01-01' # 更新键'dob'对应的值
d.pop('dob') # 删除键为'dob'的元素对
'1998-01-01'
d
{'name': 'jason', 'age': 20, 'gender': 'male'}

s = {1, 2, 3}
s.add(4) # 增加元素 4 到集合
s
{1, 2, 3, 4}
s.remove(4) # 从集合中删除元素 4
s
{1, 2, 3}

```

对字典或集合进行排序  
对于字典会根据 key 或 val 进行升序或降序排列：

```python
d = {'b': 1, 'a': 2, 'c': 10}
d_sorted_by_key = sorted(d.items(), key=lambda x: x[0]) # 根据字典键的升序排序
d_sorted_by_value = sorted(d.items(), key=lambda x: x[1]) # 根据字典值的升序排序
d_sorted_by_key
[('a', 2), ('b', 1), ('c', 10)]
d_sorted_by_value
[('b', 1), ('a', 2), ('c', 10)]

```

对于集合的排序

```python
s = {3, 4, 2, 1}
sorted(s) # 对集合的元素进行升序排序
[1, 2, 3, 4]

```

eg：
要找出这些商品有多少种不同的价格
使用列表与 set 的区别如下：

```python
# list version
def find_unique_price_using_list(products):
    unique_price_list = []
    for _, price in products: # A
        if price not in unique_price_list: #B
            unique_price_list.append(price)
    return len(unique_price_list)

products = [
    (143121312, 100),
    (432314553, 30),
    (32421912367, 150),
    (937153201, 30)
]
print('number of unique price is: {}'.format(find_unique_price_using_list(products)))

# 输出
number of unique price is: 3

```

```python
# set version
def find_unique_price_using_set(products):
    unique_price_set = set()
    for _, price in products:
        unique_price_set.add(price)
    return len(unique_price_set)

products = [
    (143121312, 100),
    (432314553, 30),
    (32421912367, 150),
    (937153201, 30)
]
print('number of unique price is: {}'.format(find_unique_price_using_set(products)))

# 输出
number of unique price is: 3

```

两者在时间复杂度上差异明显：

```python
import time
id = [x for x in range(0, 100000)]
price = [x for x in range(200000, 300000)]
products = list(zip(id, price))

# 计算列表版本的时间
start_using_list = time.perf_counter()
find_unique_price_using_list(products)
end_using_list = time.perf_counter()
print("time elapse using list: {}".format(end_using_list - start_using_list))
## 输出
time elapse using list: 41.61519479751587

# 计算集合版本的时间
start_using_set = time.perf_counter()
find_unique_price_using_set(products)
end_using_set = time.perf_counter()
print("time elapse using set: {}".format(end_using_set - start_using_set))
# 输出
time elapse using set: 0.008238077163696289

```

_Q_:  
字典的 k 可以使一个列表吗？  
eg：

```python
d = {'name': 'jason', ['education']: ['Tsinghua University', 'Stanford University']}

```

_A_:
不可以，字典的 key 要求是不可变的，而 list 是可变类型

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

### 八、异常处理

在 except block 中加入多种异常的类型有两种方式：
方式 1：

```python
try:
    s = input('please enter two numbers separated by comma: ')
    num1 = int(s.split(',')[0].strip())
    num2 = int(s.split(',')[1].strip())
    ...
except (ValueError, IndexError) as err:
    print('Error: {}'.format(err))

print('continue')
...

```

方式 2：

```python
try:
    s = input('please enter two numbers separated by comma: ')
    num1 = int(s.split(',')[0].strip())
    num2 = int(s.split(',')[1].strip())
    ...
except ValueError as err:
    print('Value Error: {}'.format(err))
except IndexError as err:
    print('Index Error: {}'.format(err))

print('continue')
...

```

若无法保证覆盖所有的异常类型，那么在最后的异常块，声明其处理的异常类型是`exception`
`exception`是所有其他异常的基类  
eg:

```python
try:
    s = input('please enter two numbers separated by comma: ')
    num1 = int(s.split(',')[0].strip())
    num2 = int(s.split(',')[1].strip())
    ...
except ValueError as err:
    print('Value Error: {}'.format(err))
except IndexError as err:
    print('Index Error: {}'.format(err))
except Exception as err:
    print('Other error: {}'.format(err))

print('continue')
...

```

或者可以在 `except` 后面省略异常类型

```python
try:
    s = input('please enter two numbers separated by comma: ')
    num1 = int(s.split(',')[0].strip())
    num2 = int(s.split(',')[1].strip())
    ...
except ValueError as err:
    print('Value Error: {}'.format(err))
except IndexError as err:
    print('Index Error: {}'.format(err))
except:
    print('Other error')

print('continue')
...

```

当程序中存在多个 except 时，最多只有一个 except 会被执行。  
若异常类型与实际相匹配，只有最前面的异常块会被执行，之后的会被忽略（找到最前面的与之匹配的异常块，然后的抛弃)  
实际的应用场景，文件读取  
eg：

```python
import sys
try:
    f = open('file.txt', 'r')
    .... # some data processing
except OSError as err:
    print('OS error: {}'.format(err))
except:
    print('Unexpected error:', sys.exc_info()[0])
finally:
    f.close()

```

这段代码中，try block 尝试读取 file.txt 这个文件，并对其中的数据进行一系列的处理，到最后，无论是读取成功还是读取失败，程序都会执行 finally 中的语句——关闭这个文件流，确保文件的完整性。因此，在 finally 中，我们通常会放一些无论如何都要执行的语句

此处可能更好的办法是使用 `with`

#### 8.1 用户自定义异常

eg:

```python
class MyInputError(Exception):
    """Exception raised when there're errors in input"""
    def __init__(self, value): # 自定义异常类型的初始化
        self.value = value
    def __str__(self): # 自定义异常类型的 string 表达形式
        return ("{} is invalid input".format(repr(self.value)))

try:
    raise MyInputError(1) # 抛出 MyInputError 这个异常
except MyInputError as err:
    print('error: {}'.format(err))

```

### 九、自定义函数

对于嵌套函数的作用：

- 9.1
- 第一，函数的嵌套能够保证内部函数的隐私。内部函数只能被外部函数所调用和访问，不会暴露在全局作用域，因此，如果你的函数内部有一些隐私数据（比如数据库的用户、密码等），不想暴露在外，那你就可以使用函数的的嵌套，将其封装在内部函数中，只通过外部函数来访问。```比如：

```python
def connect_DB():
    def get_DB_configuration():
        ...
        return host, username, password
    conn = connector.connect(get_DB_configuration())
    return conn

```

- 9.2 合理的使用函数嵌套，能够提高程序的运行效率。

```python
def factorial(input):
    # validation check
    if not isinstance(input, int):
        raise Exception('input must be an integer.')
    if input < 0:
        raise Exception('input must be greater or equal to 0' )
    ...

    def inner_factorial(input):
        if input <= 1:
            return 1
        return input * inner_factorial(input-1)
    return inner_factorial(input)


print(factorial(5))

```

这里，我们使用递归的方式计算一个数的阶乘。因为在计算之前，需要检查输入是否合法，所以我写成了函数嵌套的形式，这样一来，输入是否合法就只用检查一次。而如果我们不使用函数嵌套，那么每调用一次递归便会检查一次，这是没有必要的，也会降低程序的运行效率。

- 9.3 函数变量作用域
  不能在函数内部随意改变全局变量的值
  下面代码会报错

```python
MIN_VALUE = 1
MAX_VALUE = 10
def validation_check(value):
    ...
    MIN_VALUE += 1
    ...
validation_check(5)

```

这是因为，Python 的解释器会默认函数内部的变量为局部变量，但是又发现局部变量 MIN_VALUE 并没有声明，因此就无法执行相关操作。所以，如果我们一定要在函数内部改变全局变量的值，就必须加上 global 这个声

```python
MIN_VALUE = 1
MAX_VALUE = 10
def validation_check(value):
    global MIN_VALUE
    ...
    MIN_VALUE += 1
    ...
validation_check(5)

```

类似的，对于嵌套函数来说，内部函数可以访问外部函数定义的变量，但是无法修改，若要修改，必须加上 nonlocal 这个关键字：

```python
def outer():
    x = "local"
    def inner():
        nonlocal x # nonlocal 关键字表示这里的 x 就是外部函数 outer 定义的变量 x
        x = 'nonlocal'
        print("inner:", x)
    inner()
    print("outer:", x)
outer()
# 输出
inner: nonlocal
outer: nonlocal

```

如果不加上 nonlocal 这个关键字，而内部函数的变量又和外部函数变量同名，那么同样的，内部函数变量会覆盖外部函数的变量。

-9.4 闭包

-9.5 传递关键字参数（**kwargs）
传递**kwargs参数至另一个函数中，再继续传递
```python
    def addChnameVariable(self, list_data, **kwargs) :
        dict_names = self.getTyphoonChNameDict(**kwargs)
    
    def getTyphoonChNameDict(self, *args, **kwargs):
        nums = kwargs.get('nums')

    #外侧调用
    list_dataFinal = self.addChnameVariable(list_data, nums=list_typhoonNum)
```
如上所示，再次传递时，仍需加上**，通过`**`前缀的字典，会被解压缩为关键字参数
### 10 匿名函数

-10.1 匿名函数的定义

```python
lambda argument1, argument2,... argumentN : expression

```

简单的匿名函数：

```python
square = lambda x: x**2
square(3)

9

```

对应的方法：

```python
def square(x):
    return x**2
square(3)

9

```

lambda 可用在常规函数不能用的地方，例如列表推导

```python
[(lambda x: x*x)(x) for x in range(10)]
# 输出
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

```

注意上面的 lambda 表达式实际是一个方法，所以后面要像调用函数一样的去调用（lamdba 表达式(参数))

lambda 可以用作某些函数的参数，例如 sort

```python
l = [(1, 20), (3, 0), (9, 10), (2, -1)]
l.sort(key=lambda x: x[1]) # 按列表中元祖的第二个元素排序
print(l)
# 输出
[(2, -1), (3, 0), (9, 10), (1, 20)]

```

-10.2 lambda 主体只能是只有一行的简单表达式，而不能是多行的代码块（有一定的缺陷

-10.3 使用匿名函数的场景
(1) 减少代码的重复性
(2) 模块化代码

-10.4 python 中几个经常与匿名函数一起使用的函数：`map()`,`filter()`,`reduce()`
(1) map
首先是 map(function, iterable) 函数，前面的例子提到过，它表示，对 iterable 中的每个元素，都运用 function 这个函数，最后返回一个新的可遍历的集合。比如刚才列表的例子，要对列表中的每个元素乘以 2，那么用 map 就可以表示为下面这样：

```python
l = [1, 2, 3, 4, 5]
new_list = map(lambda x: x * 2, l) # [2， 4， 6， 8， 10]

```

注意 map 比列表推导和 fo
(2) filter
接下来来看 filter(function, iterable) 函数，它和 map 函数类似，function 同样表示一个函数对象。filter() 函数表示对 iterable 中的每个元素，都使用 function 判断，并返回 True 或者 False，最后将返回 True 的元素组成一个新的可遍历的集合。

```python
l = [1, 2, 3, 4, 5]
new_list = filter(lambda x: x % 2 == 0, l) # [2, 4]

```

(3)reduce
function 同样是一个函数对象，规定它有两个参数，表示对 iterable 中的每个元素以及上一次调用后的结果，运用 function 进行计算，所以最后返回的是一个单独的数值。

```python
l = [1, 2, 3, 4, 5]
product = reduce(lambda x, y: x * y, l) # 1*2*3*4*5 = 120

```

不过，如果你要对集合中的元素，做一些比较复杂的操作，那么，考虑到代码的可读性，我们通常会使用 for 循环，这样更加清晰明了。
在数据量非常多的情况下，比如机器学习的应用，那我们一般更倾向于函数式编程的表示，因为效率更高；
在数据量不多的情况下，并且你想要程序更加 Pythonic 的话，那么 list comprehension 也不失为一个好选择。

对一个字典，根据值进行由高及低的排序，

```python
d = {'mike': 10, 'lucy': 2, 'ben': 30}
sorted(d.items(), key=lambda x: x[1], reverse=True)
```

### 十一、面向对象

```python
class Document():
    def __init__(self, title, author, context):
        print('init function called')
        self.title = title
        self.author = author
        self.__context = context # __ 开头的属性是私有属性

    def get_context_length(self):
        return len(self.__context)

    def intercept_context(self, length):
        self.__context = self.__context[:length]

harry_potter_book = Document('Harry Potter', 'J. K. Rowling', '... Forever Do not believe any thing is capable of thinking independently ...')

print(harry_potter_book.title)
print(harry_potter_book.author)
print(harry_potter_book.get_context_length())

harry_potter_book.intercept_context(10)

print(harry_potter_book.get_context_length())

print(harry_potter_book.__context)

########## 输出 ##########

init function called
Harry Potter
J. K. Rowling
77
10

---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-5-b4d048d75003> in <module>()
     22 print(harry_potter_book.get_context_length())
     23
---> 24 print(harry_potter_book.__context)

AttributeError: 'Document' object has no attribute '__context'

```

类：一群有着相似性的事物的集合，这里对应 Python 的 class。  
对象：集合中的一个事物，这里对应由 class 生成的某一个 object，比如代码中的 harry_potter_book。  
属性：对象的某个静态特征，比如上述代码中的 title、author 和 \_\_context。  
函数：对象的某个动态能力，比如上述代码中的 intercept_context () 函数。

```python
class Document():

    # 用大写来表示常量
    WELCOME_STR = 'Welcome! The context for this book is {}.'

    def __init__(self, title, author, context):
        print('init function called')
        self.title = title
        self.author = author
        self.__context = context

    # 类函数
    # 注意类函数的第一个参数通常命名为cls，表示传入一个类
    @classmethod
    def create_empty_book(cls, title, author):
        return cls(title=title, author=author, context='nothing')

    # 成员函数
    def get_context_length(self):
        return len(self.__context)

    # 静态函数
    # 静态函数与类并没有什么关联，所以第一个参数没有任何特殊性
    @staticmethod
    def get_welcome(context):
        return Document.WELCOME_STR.format(context)


empty_book = Document.create_empty_book('What Every Man Thinks About Apart from Sex', 'Professor Sheridan Simove')


print(empty_book.get_context_length())
print(empty_book.get_welcome('indeed nothing'))

########## 输出 ##########

init function called
7
Welcome! The context for this book is indeed nothing.

```

对于类变量，在内部可以通过`self.WELCOME_STR`的方式访问；  
在外部可以通过 Entity.WELCOME_STR 的方式获取。
对于类函数而言，常用的场景是用来实现不同的`init`构造函数，如上例所示.

-10.2 继承
继承类在被创建时，是不会自动调用父类的构造函数的。必须在`init`函数中显示的调用父类的构造函数。  
调用的执行顺序是 子类 init->父类 init

(1)

```python
class Entity():
    def __init__(self, object_type):
        print('parent class init called')
        self.object_type = object_type

    def get_context_length(self):
        raise Exception('get_context_length not implemented')

    def print_title(self):
        print(self.title)

class Document(Entity):
    def __init__(self, title, author, context):
        print('Document class init called')
        Entity.__init__(self, 'document')
        self.title = title
        self.author = author
        self.__context = context

    def get_context_length(self):
        return len(self.__context)

class Video(Entity):
    def __init__(self, title, author, video_length):
        print('Video class init called')
        Entity.__init__(self, 'video')
        self.title = title
        self.author = author
        self.__video_length = video_length

    def get_context_length(self):
        return self.__video_length

harry_potter_book = Document('Harry Potter(Book)', 'J. K. Rowling', '... Forever Do not believe any thing is capable of thinking independently ...')
harry_potter_movie = Video('Harry Potter(Movie)', 'J. K. Rowling', 120)

print(harry_potter_book.object_type)
print(harry_potter_movie.object_type)

harry_potter_book.print_title()
harry_potter_movie.print_title()

print(harry_potter_book.get_context_length())
print(harry_potter_movie.get_context_length())

########## 输出 ##########

Document class init called
parent class init called
Video class init called
parent class init called
document
video
Harry Potter(Book)
Harry Potter(Movie)
77
120

```

(2)抽象函数和抽象类：

```python
from abc import ABCMeta, abstractmethod

class Entity(metaclass=ABCMeta):
    @abstractmethod
    def get_title(self):
        pass

    @abstractmethod
    def set_title(self, title):
        pass

class Document(Entity):
    def get_title(self):
        return self.title

    def set_title(self, title):
        self.title = title

document = Document()
document.set_title('Harry Potter')
print(document.get_title())

entity = Entity()

########## 输出 ##########

Harry Potter

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-7-266b2aa47bad> in <module>()
     21 print(document.get_title())
     22
---> 23 entity = Entity()
     24 entity.set_title('Test')

TypeError: Can't instantiate abstract class Entity with abstract methods get_title, set_title

```

抽象类是一种特殊的类，它生下来就是作为父类存在的，一旦对象化就会报错。同样，抽象函数定义在抽象类之中，子类必须重写该函数才能使用。相应的抽象函数，则是使用装饰器 @abstractmethod 来表示。
