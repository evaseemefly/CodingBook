# vue 的一些基本应用的整理
## 1 
### 1.1 绑定下拉框

```html
<select class="from-control col-md-7" v-model="level">
    <option v-for="item in typhoonLevelOptions" :key="item.key"
        :value="item.key">{{item.name}}
    </option>
</select>
```
vue绑定下拉框较为简单，主要注意两点，一个是selecl中的`v-model='level'`实现选择修改level的值；
在option中，必须要指定key

### 1.2 动态绑定class与样式  
[参考自官方说明](https://cn.vuejs.org/v2/guide/class-and-style.html)
[其他的文章](https://www.cnblogs.com/big-snow/p/5718728.html)  

摘自别人的一段话：
> Vue.js 的核心是一个响应的数据绑定系统，它允许我们在普通 HTML 模板中使用特殊的语法将 DOM “绑定”到底层数据。被绑定的DOM 将与数据保持同步，每当数据有改动，相应的DOM视图也会更新。基于这种特性，通过vue.js动态绑定class就变得非常简单。  


数据绑定和正常的一样使用`v-bind:属性名`简写为`:属性名`
v-bind:class 支持数据变量，当变量值改变时，将同时更新class。v-bind:class指令的值限定为绑定表达式，如javascript表达式  
注意：v-bind:class 同样支持string类型，因为string是固定不变的，不建议这样使用。

方式1：
```html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```
```js
data: {
  isActive: true,
  hasError: false
}
```
最终的渲染结果为：
```html
<div class="static active"></div>
```

方式2：
数组的方式
```html
<div :class="[activeClass, errorClass]"></div>
```

```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
最终渲染为：
```html
<div class="active text-danger"></div>
```

方式3：
三元表达式
```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```