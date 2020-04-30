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
//数组语法： 在你数组里面放对象
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
  //item这里可以增加index参数，代表索引 (item,index)
  <li v-for="item in items" :key="item.message">  //冒号是v-bind的缩写
    {{ item.message }}
  </li>
</ul>
data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]}
```

#### 事件函数

- 在导出对象下添加`methods`属性，在里面写上事件函数，事件函数只能写成普通函数，只有这里才能使用this(只有普通函数能拿到this)

- 这里面定义的函数不一定都当作事件函数，也可以写普通函数

#### 修改data

1. 直接修改，不用setState之类的，`this.num++`
2. 事件函数要写成普通函数，这样才可以用this
   - 综述： 在template中，直接修改，在script中，使用this.xxx来修改

#### 事件绑定（监听）

`v-on`，简写@  `@click="函数或者函数内的内容"`

1. 事件绑定的时候可以赋值一个函数，点击的时候函数触发(事件函数要写成普通函数，这样才可以用this)

2. 将执行的函数内的内容直接写到click内，点击的时候执行，一般执行的内容都是简单的修改数据

3. 事件传参： (直接传递)

   - 事件函数传参，直接在函数调用的时候在后面的括号里写上参数（不必像React那样写箭头函数传）；

   - 如果事件函数设置了参数，那么如果事件触发的时候不传递参数 ，那么事件函数接收的第一个参数默认是`event`事件对象；
   - 想传事件对象的话，使用`$event`来传递`@click=add(100,$event)`

#### 表单

>  数据的双向绑定：1. 当用户输入内容时导致视图改变；2. 通过js逻辑处理修改data，导致视图改变

> 表单内的submit提交按钮点击的时候默认会清空表单，需要在事件函数阻止：接收event参数，`event.preventDefault `  另外还有`event.stopPropagation()`阻止事件冒泡

`<input type = "text" :value='value'/>`

1. 写上`@change = 'handleInput'`，change是在失去焦点的时候触发的（不是事实的）

2. ` @input='handleInput'`事件是实时改变的
3. `v-model = 'value'`： 替换监听函数和默认值  **推荐**

#### this指向问题

1. 普通函数的this指的是vue组件，一般在对象里面写普通函数，
2. 箭头函数的this是在函数创建的时候去找

2. 导出的是对象，对象里面写箭头函数会出现问题，找不到this（因为在vue中，是导出对象而不是class）

3. 而React是class组件，里面的this指向就是该组件
   - 综述：在class中，使用箭头函数，在对象中，使用普通函数；

#### 表单的输入绑定

`v-model`  直接使用 v-model 指令，搭配value使用

```js
//复选：  				checkArr是data里的空数组
<label for='vue'>vue</label><input id="vue" value="vue" type="checkbox" v-model="checkArr"/>    <label for='react'>react</label><input id="react" value="react" type="checkbox" v-model="checkArr"/>
 //单选:
  <input type="radio" id="one" value="One" v-model="picked"> //picked为data里的一个属性，默认空
  <label for="one">One</label>
//选择框
  <select v-model="selected"> //selected为一个默认为空的data属性
    <option disabled value="">请选择</option>
    <option>A</option>   //可以增加value属性，这样的话值以value为准
    <option>B</option>
    <option>C</option>
  </select>
```

>  值绑定：修改value的默认内容<input  type="checkbox"  v-model="toggle"  true-value="yes"  false-value="no" >在script中：`// 当选中时 vm.toggle === 'yes' // 当没有选中时 vm.toggle === 'no'`

#### 修饰符

##### 事件修饰符 (v-on)

- `.stop` 阻止事件冒泡

- `.prevent` 阻止时间的默认行为（a的跳转，表单的提交）
- `.capture` 颠倒事件触发顺序
- `.self` 只有当event.target 是当前元素自身时触发处理函数，只在自身触发，下级不触发（优于阻止冒泡）
- `.once` 事件只触发一次
- `.passive` 新增的，移动端滑动优化

##### 按键修饰符

> <input v-on:keyup.enter="submit">
>
> <input v-on:keyup.13="submit">  //也可以使用按键码

`.enter` `.tab` `.delete` `.esc` `.space` `.up` `.left`

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

`.ctrl`  `.alt` `.shift` `.meta`

在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。

##### v-model 修饰符

- `.lazy` 使用懒惰模式：双向绑定不使用input事件而是使用change事件（不是事实修改的）
- `.number`转换为数字（默认输入的数字是String而不是Number）
- `.trim`删除首尾空格

#### 计算属性

> 由于计算属性存在缓存，不是每一次都会执行，所以比方法好
>
> 计算属性是依赖缓存的，一个计算属性所依赖的数据发生变化时，才会重新取值，所以只要内容不变，计算属性也就不会跟新；
>
> 使用方法的话，只要数据一发生变化，就会重新调用methods，开销较大

1. 计算属性：将data内的数据进行变形，相当于创建一个新的data

2. 用法： 在导出对象内添加一个新的属性 `computed` 属性（一个对象），该对象下只能添加方法，每一个方法代表一个计算属性；该方法必须设置返回值，返回新的data，该data只读（只能用，不能改），而且不传参，值从data里拿：

   注意：计算属性的名不能和data的名冲突

```js
computed:{
  reveseStr(){
    return this.text.split('').reverse().join('')
  }
}
```

3. 计算属性的setter

> 直接修改计算属性会失败，提示需要给计算属性设置一个setter，把计算属性写成对象类型，里面包含两个属性set和get，

```js
computed:{
  num:{
    get(){					//get获取数据，直接修改计算属性时该计算属性的set会被触发
      return this.num+1
    },
    set(val){				//set用于反向修改，去修改data里的属性(会修改数据源头)
      this.num = val-1
    }
  }
}
```

#### 侦听器

`watch:{}`也是导出列表下的一个属性，监听组件内的data或者props变化，从而控制另外的data

```js
watch:{
  num(val){				//这里的num和data里的num同名
    this.num = val
  }
```

当需要在数据变化时执行异步或开销较大的操作时，使用watch。

computed需要写返回值，而异步操作设置返回值不能同步执行，watch只是监听，并不需要写返回值

#### 指令v-



|     指令      | 功能                                                         |               示例               | 备注 | 备注   |
| :-----------: | :----------------------------------------------------------- | :------------------------------: | :--: | ------ |
|    v-bind     | 在template中写vue语法，绑定数据                              |        v-bind:title="msg"        |  ：  |        |
|    v-text     | 更新元素的 textContent                                       |      <h1 v-text='msg'></h1>      |      |        |
|    v-html     | 更新元素的 innerHTML                                         |      <h1 v-html='msg'></h1>      |      |        |
|     v-on      | 绑定事件（可以使用各种修饰符）                               | v-on:click="say('参数', $event)" |  @   |        |
|    v-model    | 在表单元素上创建双向数据绑定                                 | <input type="text" v-model="a"/> |      |        |
|     v-for     | 基于源数据多次渲染元素或模板块                               |                                  |      |        |
| v-if & v-show | v-if：销毁或重建元素  v-show：根据表达式之真假值，切换元素的 display CSS 属性 |                                  |      |        |
|     v-pre     | 跳过这个元素和它的子元素的编译过程                           |       <p v-pre>{{Arr}}</p>       |      |        |
|    v-once     | 只渲染元素和组件一次。                                       |                                  |      |        |
|    v-cloak    | 这个指令保持在元素上直到关联实例结束编译                     |                                  |      | 还没学 |
|    v-slot     | 插槽，传递children用的                                       |          <a v-slot:msg>          |  #   |        |

### 数据传递

#### prop

>  当子组件在父组件中展示的时候，可能需要多个相同的子组件，但是内容或者样式不同，此时需要父组件向子组件传递prop，子组件接收之后，根据传过来的数据进行改变

```js
//在导出里增加props属性，该属性是数组的时候:
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
//是对象的时候:
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
//带有默认值的时候：
props:{
    text:{
      default:'默认按钮',
      required:true,   //required必须传参（当然就不需要写上面的默认值了）
      type:String 
     }}
```

#### 自定义事件

> prop 是父组件向子组件传递数据
>
> 自定义事件是父组件向子组件传递函数

接收： 在template中，使用`$emit('事件名')`接收；在script中，使用`this.$emit('事件名')`接收

```js
<Button :text="num" color="red" @add = 'add'/>  //template中
  methods:{   //script中的methods属性中定义方法
    add(){
      this.num++
    }}} //在子组件中接收：
<button :style="{color: color}" @click="$emit('add')">{{text}}</button>
```

综述：父组件给子组件传参，可以使用prop和自定义事件，定义方式不同，用法基本一样。

#### 插槽 slot

`v-slot`简写成`#`

在父组件中使用子组件时向子组件的双闭合标签内传递的参数（类似于React的`children`）

在子组件中使用`<slot></slot>`

假如父组件传递的插槽内容想要在子组件中的不同位置展示，那么就需要定义多个插槽（需要给插槽命名）：

具名插槽 

```js
<template v-slot:msg> //父组件传递 简写成 <template #msg>
    <h3>具名插槽</h3>
</template>
<slot></slot>  //子组件默认接收
<slot name="msg"></slot>   //子组件接收具名插槽
```



### 冷知识

`$nextTick`用来知道什么时候DOM更新完成