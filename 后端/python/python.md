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
对于字典会根据key或val进行升序或降序排列：  
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
    使用列表与set的区别如下：

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

*Q*:  
字典的k可以使一个列表吗？  
eg：
```python
d = {'name': 'jason', ['education']: ['Tsinghua University', 'Stanford University']}

```
*A*:
不可以，字典的key要求是不可变的，而list是可变类型
