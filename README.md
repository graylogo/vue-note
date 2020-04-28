### 安装

1. 全局安装脚手架 `npm install -g @vue/cli` 或者`yarn global add @vue/cli`
2. 使用脚手架创建项目 `vue create hello-world`，选择default安装

### 语法

#### 模板语法

1. 使用双花括号：在闭合标签中使用js，用双括号包裹

2. 使用``v-bind:``指令： 用js控制开始标签内的属性，简写成冒号 : 就可以直接在引号内添加js

   `<div v-bind:id="dynamicId"></div>`

3. `v-html` 用来插入html标签

   ```js
   <h2><span v-html="num"></span></h2>  num是data里的变量，将变量的值解析进span标签里
   ```

#### 组件的嵌套

> 组件的后缀为.vue，组件一般包含三个部分：*template* *script* *style*
>
> 给style标签增加<style scoped>属性，css样式只在这个组件生效

在父组件中子组件，在父组件导出对象下添加一个components对象，里面写上导入的名字就可以在template中使用了。

#### 组件的数据data

定义： 直接在默认导出的对象下增加data属性，该属性的值是函数，需要返回一个**对象**

- 三种定义方法：

> ```js
> data(){
>   return{
>     num:10
>   }}
> ```

> ```js
> data:()=>{
>   return{
>     num:10
>   }}
> ```

> ```js
> data:function () {
> 	return{
>   	num:10
> }}
> ```

#### Class绑定

1. 直接使用模版语法判断v-bind

   ```js
   <h2 :class="num?'box-show':'box'">{{num}}</h2>
   ```

2. 对象语法

   ```js
   <h2 :class="{ active: isActive, 'text-danger': hasError }">{{num}}</h2>
   ```

3. 数组语法

   ```js
   <h2 :class="['box',show?'show':'']">{{num}}</h2>
   ```

#### Style绑定

```js
<h3 style="color: red">style</h3> 	默认写法
<h3 :style="`color:${show?'red':'black'}`">style</h3>  带判断的，模板字符串
<h3 :style="{display:show?'block':'none'}">a</h3>			对象语法
数组语法： 在你数组里面放对象
```

#### 条件渲染

1. 样式控制（none｜block｜flex｜table...） v-show

   ```js
   <h1 v-show="show">Hello!</h1>   反复出现(下拉菜单等)
   ```

2. 结构控制（标签是否存在）v-if

   ```js
   <h1 v-if="show">Hello!</h1>  骨架瓶之类
   <h1 v-else-if="num">Hello!</h1>   v-else-if增加第三个判断
   <h1 v-else>Hello!</h1>  v-else配合v-if使用，保证兄弟级，前后紧跟
   ```

#### 列表渲染 v-for

```js
<ul id="example-1">
  item这里可以增加index参数，代表索引 (item,index)
  <li v-for="item in items" :key="item.message">  冒号是v-bind的缩写
    {{ item.message }}
  </li>
</ul>
data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
```

#### 事件函数

在导出对象下添加`methods`属性，在里面写上事件函数，事件函数只能写成普通函数，只有这里才能使用this

这里面定义的函数不一定都当作事件函数，也可以写普通函数(只有普通函数能拿到this)

#### 修改data

1. 直接修改，不用setState之类的，`this.num++`
2. 事件函数要写成普通函数，这样才可以用this
   - 综述： 在template中，直接修改，在script中，使用this.xxx来修改

#### 事件绑定（监听）

`v-on`，简写@  `@click="函数或者函数内的内容"`

1. 事件绑定的时候可以赋值一个函数，点击的时候函数触发(事件函数要写成普通函数，这样才可以用this)
2. 将执行的函数内的内容直接写到click内，点击的时候执行，一般执行的内容都是简单的修改数据
3. 事件传参： 如果事件函数设置了参数，那么如果事件触发的时候不传递参数 ，那么事件函数接收的第一个参数默认是`moseevent`事件对象；事件函数传参，直接在函数调用的时候在后面的括号里写上参数（不必像React那样写箭头函数传）；想传事件对象的话，使用`$event`来传递

#### 表单

>  数据的双向绑定：1. 当用户输入内容时导致视图改变；2. 通过js逻辑处理修改data，导致视图改变

> 表单内的submit提交按钮点击的时候默认会清空表单，需要在事件函数阻止：接收event参数，`event.preventDefault`

`<input type = "text" :value='value'/>`

1. 写上`@change = 'handleInput'`，change是在失去焦点的时候触发的（不是事实的）

2. ` @input`事件是实时改变的
3. `v-model = 'value'`： 替换监听函数和默认值



#### this指向问题

1. this 普通函数的this指的是vue组件，一般在对象里面写普通函数，

2. 导出的是对象，对象里面写箭头函数会出现问题，找不到this（因为在vue中，是导出对象而不是class）

3. 而React是class组件，里面的this指向就是该组件
   - 综述：在class中，使用箭头函数，在对象中，使用普通函数；

#### 安德森

