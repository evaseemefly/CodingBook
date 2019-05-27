备份汇总的知识点在  
[我在这](https://github.com/evaseemefly/go-/tree/master/goinaction/code/chapter2)  
以后会迁移至此

## 第二章：基本程序结构  
### 05 变量、常量以及其他语言的差异
### 06 数据类型  
类型转化：  
与其他语言的差异  
1. 够不支持隐式类型转换
2. 别名和原有类型也不能进行隐式类型转换  

指针类型  
1. 不支持指针运算
2.  string是值类型，其默认的初始化值为空字符串('')，而不是`nil`

注意`&`是指针运算符，他代表的是这个值的地址，*不可以直接操作指针地址*
```go
    a := 1
	aPtr := &a
	//aPtr = aPtr + 1
	t.Log(a, aPtr)
	t.Logf("%T %T", a, aPtr)
```

```
    type_test.go:20: 1 0xc00009e130
    type_test.go:21: int *int
```
### 07 运算符  
其他语言的== 比较运算符比较的是引用，  
而go中  

### 08 条件及循环  
```go
func TestSwitchMultiCase(t *testing.T) {
	for i := 0; i < 5; i++ {
		switch i {
		case 0, 2:
			t.Log("Even")
		case 1, 3:
			t.Log("Odd")
		default:
			t.Log("it is not 0-3")
		}
	}
}

func TestSwitchCaseCondition(t *testing.T) {
	for i := 0; i < 5; i++ {
		switch {
		case i%2 == 0:
			t.Log("Even")
		case i%2 == 1:
			t.Log("Odd")
		default:
			t.Log("unknow")
		}
	}
}
```  
```
=== RUN   TestSwitchMultiCase
--- PASS: TestSwitchMultiCase (0.00s)
    condition_test.go:9: Even
    condition_test.go:11: Odd
    condition_test.go:9: Even
    condition_test.go:11: Odd
    condition_test.go:13: it is not 0-3
=== RUN   TestSwitchCaseCondition
--- PASS: TestSwitchCaseCondition (0.00s)
    condition_test.go:22: Even
    condition_test.go:24: Odd
    condition_test.go:22: Even
    condition_test.go:24: Odd
    condition_test.go:22: Even
PASS
```
### 9 map
在访问的key不存在时，仍会返回默认值-0，不能通过返回`nil`来判断元素是否存在  
1. map的创建  
    注意map使用make创建时，不可以向数组与切片一样指定步长。  
    因为指定步长后会默认为其分配默认值为0的值，则会出错
```go
package my_map

import "testing"

func TestInitMap(t *testing.T) {
	m1 := map[int]int{1: 1, 2: 4, 3: 9}
	t.Log(m1[2])
	t.Logf("len m1=%d", len(m1))
	m2 := map[int]int{}
	m2[4] = 16
	t.Logf("len m2=%d", len(m2))
	m3 := make(map[int]int, 10)
	t.Logf("len m3=%d", len(m3))
}
```
```
=== RUN   TestInitMap
--- PASS: TestInitMap (0.00s)
    map_test.go:7: 4
    map_test.go:8: len m1=3
    map_test.go:11: len m2=1
    map_test.go:13: len m3=0
=== RUN   TestAccessNotExistingKey
```
2. 判断map中指定key是否存在，若直接根据指定key去取，若不存在则会自动创建一个该key对应的默认值0；  
   所以不可使使用取值后判断是否为0的方式来判断map中是否包含指定key，而应该通过如下方式进行判断
```go
func TestAccessNotExistingKey(t *testing.T) {
	m1 := map[int]int{}
	t.Log(m1[1])
	m1[2] = 0
	t.Log(m1[2])
	m1[3] = 0
	// 对于map，若判断指定的key是否存在对应的value，第二个参数返回的是是否存在，可以通过如下方式进行判断
	if v, ok := m1[3]; ok {
		t.Logf("Key 3's value is %d", v)
	} else {
		t.Log("key 3 is not existing.")
	}
}
```

```

--- PASS: TestAccessNotExistingKey (0.00s)
    map_test.go:18: 0
    map_test.go:20: 0
    map_test.go:24: Key 3's value is 0
=== RUN   TestTravelMap
```
3. 使用range进行遍历，分别取出key与value
```go
func TestTravelMap(t *testing.T) {
	m1 := map[int]int{1: 1, 2: 4, 3: 9}
	for k, v := range m1 {
		t.Log(k, v)
	}
}

```
```
--- PASS: TestTravelMap (0.00s)
    map_test.go:33: 1 1
    map_test.go:33: 2 4
    map_test.go:33: 3 9
PASS
```