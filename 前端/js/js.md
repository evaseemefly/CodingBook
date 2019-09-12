## 1 常用的库

## 1.1 date

### 1.1.1 对日期进行加减法

js 中对日期进行加减法的思路有：

1. 将 date 转成时间戳（单位毫秒）
   将 date 转成时间戳，通过`.setTime`直接加毫秒\*n
   eg:

   ```js
   var t = new Date(); //你已知的时间
   var t_s = t.getTime(); //转化为时间戳毫秒数

   t.setTime(t_s + 1000 * 60); //设置新时间比旧时间多一分钟
   alert(t); // 2016-12-11 20:21:20

   t.setTime(t_s + 1000 * 60 * 60); //设置新时间比旧时间多一小时
   alert(t); // 2016-12-11 21:20:20

   t.setTime(t_s + 1000 * 60 * 60 * 24); //设置新时间比旧时间多一天
   ```

   ```js
   var t = new Date(); //你已知的时间

   t.setTime(t.setMinutes(t.getMinutes() + 1)); //设置新时间比旧时间多一分钟
   alert(t); // 2016-12-11 20:21:20

   t.setTime(t.setHours(t.getHours() + 1)); //设置新时间比旧时间多一小时
   ```

2. 直接使用`setHours`,`setMinutes`,`setSeconds`等
   eg:

```js
var date = new Date();
date.setHours(25);
date.setMinutes(61);
date.setSeconds(61);
// 超过的时间会自动进位，所以可以直接使用Date对象进行加减运算
date.setHours(date.getHours() + 1); // 当前时间加1小时
date.setHours(date.getHours() - 1); // 当前时间减1小时
date.setHours(date.getHours() + 1.9); // 小数会自动取整，即只增加1个小时
```

### 1.1.2 对 date 进行 format

建议使用第三方的库

[dateformat](https://www.npmjs.com/package/dateformat)

# 2 数组的操作

## 2.1 快速生成一个数组

[参考](https://juejin.im/entry/58d54d0da22b9d0064597c30)
eg：创建一个长度为 100 的数组
传统 for 循环
来说一下传统的 for 循环：

```js
var arr = new Array(100);
for (var i = 0; i < arr.length; i++) {
  arr[i] = i;
}
arr; // [0, 1, ..., 99]
```

通过 es6 实现
普通实现
ES6 中数组实例有 keys、values、entries 方法，分别用于遍历数组的索引、键值、键值对，它们都返回遍历器对象（详细 👉Iterator 和 for…of 循环）

因此我们可以用 ES6 的 Array.from 转成数组：

```js
Array.from(new Array(100).keys());
```

优雅进阶 - 扩展运算符
ES6 新增 ... 扩展运算符，极大方便了相关操作(详见 👉 函数的扩展里面的扩展运算符)

因此我们可以更加优雅地实现：

```js
[...Array(100).keys()];
```

或者

```js
[...Array.from({ length: 100 }).keys()]
[...Array(100).keys()];
```

## 2.2 将 nodelist 转成数组

eg：

```js
// 找到第一个子div设置left的偏移
var child: HTMLElement = calendarDom.childNodes[1] as HTMLElement;
var arr = calendarDom.childNodes;

let tempArr = Array.from(arr);
```

主力里面的 dom 元素的子节点，类型是`nodelist`类型，并不是 array，需要通过`Array.from()`转成数组

[参考](https://gomakethings.com/converting-a-nodelist-to-an-array-with-vanilla-javascript/)
