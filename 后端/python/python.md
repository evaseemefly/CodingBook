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
