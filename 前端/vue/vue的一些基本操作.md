# vue 的一些基本应用的整理

## 1

### 1.1 绑定下拉框

```html
<select class="from-control col-md-7" v-model="level">
  <option v-for="item in typhoonLevelOptions" :key="item.key" :value="item.key"
    >{{item.name}}
  </option>
</select>
```

vue 绑定下拉框较为简单，主要注意两点，一个是 selecl 中的`v-model='level'`实现选择修改 level 的值；
在 option 中，必须要指定 key

### 1.2 动态绑定 class 与样式

[参考自官方说明](https://cn.vuejs.org/v2/guide/class-and-style.html)
[其他的文章](https://www.cnblogs.com/big-snow/p/5718728.html)

摘自别人的一段话：

> Vue.js 的核心是一个响应的数据绑定系统，它允许我们在普通 HTML 模板中使用特殊的语法将 DOM “绑定”到底层数据。被绑定的 DOM 将与数据保持同步，每当数据有改动，相应的 DOM 视图也会更新。基于这种特性，通过 vue.js 动态绑定 class 就变得非常简单。

数据绑定和正常的一样使用`v-bind:属性名`简写为`:属性名`
v-bind:class 支持数据变量，当变量值改变时，将同时更新 class。v-bind:class 指令的值限定为绑定表达式，如 javascript 表达式  
注意：v-bind:class 同样支持 string 类型，因为 string 是固定不变的，不建议这样使用。

方式 1：

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

方式 2：
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

方式 3：
三元表达式

```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

### 1.3 css 作用域

[部分摘自](https://www.w3cplus.com/vue/scoped-styles-vs-css-modules.html)

简单的小结一下，在 Vue 中 scoped 属性的渲染规则：

- 作用域 css  
  给 DOM 节点添加一个不重复的 data 属性（比如 data-v-7ba5bd90）来表示他的唯一性  
  在每个 CSS 选择器末尾（编译后生成的 CSS）加一个当前组件的 data 属性选择器（如[data-v-7ba5bd90]）来私有化样式。选择器末尾的 data 属性和其对应的 DOM 中的 data 属性相匹配  
  如果组件内部包含有其他组件，只会给其他组件的最外层标签加上当前组件的 data 属性  
  上面我们看到的是 Vue 机制内作用域 CSS 的使用。在 Vue 中，除了作用域 CSS 之外，还有另外一种机制，那就是 CSS Modules，即模块化 CSS。

- css Modules
