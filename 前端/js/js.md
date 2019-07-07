## 1 常用的库
## 1.1 date
### 1.1.1 对日期进行加减法

js中对日期进行加减法的思路有：
1. 将date转成时间戳（单位毫秒）
   将date转成时间戳，通过`.setTime`直接加毫秒*n
   eg:
   ```js
   var t = new Date();//你已知的时间
   var t_s = t.getTime();//转化为时间戳毫秒数
 
   t.setTime(t_s + 1000 * 60);//设置新时间比旧时间多一分钟
   alert(t) // 2016-12-11 20:21:20
 
   t.setTime(t_s + 1000 * 60 * 60);//设置新时间比旧时间多一小时
   alert(t) // 2016-12-11 21:20:20
 
   t.setTime(t_s + 1000 * 60 * 60 * 24);//设置新时间比旧时间多一天
   ```

   ```js

    var t = new Date();//你已知的时间
 
    t.setTime(t.setMinutes(t.getMinutes() + 1));//设置新时间比旧时间多一分钟
    alert(t) // 2016-12-11 20:21:20
 
    t.setTime(t.setHours(t.getHours() + 1));//设置新时间比旧时间多一小时
   ```

2. 直接使用`setHours`,`setMinutes`,`setSeconds`等
eg:
```js
var date = new Date();
date.setHours(25);
date.setMinutes(61);
date.setSeconds(61);
// 超过的时间会自动进位，所以可以直接使用Date对象进行加减运算
date.setHours(date.getHours() + 1);    // 当前时间加1小时
date.setHours(date.getHours() - 1);    // 当前时间减1小时
date.setHours(date.getHours() + 1.9);    // 小数会自动取整，即只增加1个小时
```