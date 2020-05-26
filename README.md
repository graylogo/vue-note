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

#### 修改data中的数组

> 有时候数组的更改不能触发视图的更新

- 数据是一个对象，更新时，给对象添加新的属性，不能触发视图的更新（但是数据实际时更新了），解决办法：重新赋值一个新的对象（直接把对象修改了）
- 使用`$set()`方法` $set(obj,"age",10)`添加age=10这一条属性
- 数据是对象数组，给该对象新增属性

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

1. 写上`@change = 'handleInput'`，change是在失去焦点的时候触发的（不是时实的）

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

.sync 将props变为双向绑定，可以支持子组件修改父组件prop

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

> 由于计算属性存在缓存，不是每一次都会执行，所以比方法好（计算属性是特定的一个属性，不能传参）
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

#### 过滤器

类似过滤器

```js
filters: {   //在导出对象中
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1) //将首字母大写
  }}
<!-- 在双花括号中使用 -->
{{ message | capitalize }}
<!-- 在 `v-bind` 中使用 -->
<div v-bind:id="rawId | formatId"></div>
//全局过滤器
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
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
| v-if & v-show | v-if：销毁或重建元素  v-show：根据表达式之真假值，切换元素的 display CSS 属性 |               区别               |      |        |
|     v-pre     | 跳过这个元素和它的子元素的编译过程                           |       <p v-pre>{{Arr}}</p>       |      |        |
|    v-once     | 只渲染元素和组件一次。                                       |                                  |      |        |
|    v-cloak    | 这个指令保持在元素上直到关联实例结束编译                     |                                  |      | 还没学 |
|    v-slot     | 插槽，传递children用的                                       |          <a v-slot:msg>          |  #   |        |

v-if从出现到消失的生命周期

v-if从小事到出现的生命周期

v-show不产生生命周期

#### 自定义指令

全局指令

````js
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()}})
````

局部指令

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

接收： 在template中，使用`$emit('事件名','参数')`接收；在script中，使用`this.$emit('事件名')`接收

```js
<Button :text="num" color="red" @add = 'add'/>  //template中
  methods:{   //script中的methods属性中定义方法
    add(){
      this.num++
    }}} //在子组件中接收：
<button :style="{color: color}" @click="$emit('add')">{{text}}</button>
//在methods中，使用this.$emit('add')接收
```

- 在事件函数里（绑定各种事件的函数）可以用事件函数 $event...  必须在绑定的部分才能用，要在methods里使用的话，在上面和下面都传&event
- $emit() 自定义事件传参会存在问题，传多次的时候无法正确获取到参数。因为传递的时候直接将执行了的函数装到另外一个函数里面，所以要在第一次传递的时候就需要传参

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

#### 动态组件

`keep-alive` 失活的组件将会被缓存,被记住状态

```js
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
//在一个多标签的界面中使用 :is attribute 来切换不同的组件：
<component v-bind:is="currentTabComponent"></component>
在data中： currentTabComponent:'标签名'
在点击事件中，修改currentTabComponen="另外一个标签名"
```

注意：动态组件的is相当于v-if，但是它有缓存

is的用法在脚手架中用不了？？？



### 生命周期

mounted

```js
created(){//实例创建完成，初始化事件和数据完毕}  改数据
mounted() {//初始挂载，在浏览器中出现组件的页面结构}
updated(){//更新data或者prop，并且页面渲染完毕}
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gebmnzb4mqj30u023ztaj.jpg" style="zoom:35%;" />

### 动画

Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。包括以下工具：

- 在 CSS 过渡和动画中自动应用 class
- 可以配合使用第三方 CSS 动画库，如 Animate.css
- 在过渡钩子函数中使用 JavaScript 直接操作 DOM
- 可以配合使用第三方 JavaScript 动画库，如 Velocity.js

vue内置了translation组件来实现组件的动画过渡和动画效果：

1. 用translation标签包裹动画组件

   ```js
   <transition name="fade">  //加入appear后初始的时候会执行（当然也可以自定义）
     <p v-if="show">hello</p>
   </transition>
   ```

2. 在style里面设置动画

   ```css
   .fade-enter-active, .fade-leave-active { //xxx-enter-active 出现的过程  xxx代表上面的name
     transition: opacity .5s;}
   .fade-enter, .fade-leave-to  {
     opacity: 0;} //xxx-enter出现的第一祯		xxx-enter-to出现结束的最后一帧
   ```

3. 如果元素默认是出现的，则消失的第一祯和出现的最后一帧不需要再写了；

   反之，如果默认消失，则出现的第一祯和消失的最后一帧不需要设置。

4. 动画

   ```css
   .bounce-enter-active {
    animation: bounce-in .5s;}//:duration="{ enter: 500, leave: 800 }"可以在template里设置时间
   .bounce-leave-active {
     animation: bounce-in .5s reverse;}  //reverse 反向
   @keyframes bounce-in {
     0% {transform: scale(0);}
     50% {transform: scale(1.5);}
     100% {transform: scale(1);}}
   ```

### 获取焦点（补全）

- `$nextTick`用来知道什么时候DOM更新完成
- 通过ref获取真实DOM节点
- 获取焦点的时候用settimeout 0 来改成异步的

- document.activrElement   激活状态下的元素，例如选中的checkbox，获得焦点的input

### Axios请求

vue引入组件的三种方式：

1. 在否个组件中引入并使用

2. 使用vue.use方法将该插件当成全局插件，在每个组件中都能用（在main.js）中使用

   ```js
   npm i axios vue-axios  //装包
   import axios from 'axios';  //在main.js中导入并设置为全局
   import Vueaxios from 'vue-axios'
   Vue.use(Vueaxios,axios) //设置为全局
   this.axios.get(api).then((response) => { //在组件中使用
     console.log(response.data)})
   ```

3. 假如导入的第三方插件不是组件形式的，可以使用Vue.prototype将该插件定义成全局变量（在main.js中），`Vue.prototype.axios=axios`在组件内可以使用this.xxx来访问

> 使用axios发送请求  还有vue-axios

在created生命周期发送请求拿数据

并发请求，使用`axios.all`和`spread`来解决

```js
axios.all([searchTopic(), searchs()])
  .then(axios.spread(function (allSearchTopic, allSearchs) { //可以用箭头函数
    debugger//打印可以拿到所有的返回值
    allSearchTopic == 方法一的返回值
    allSearchs == 方法二的返回值
  }));
```

判断输入内容发送请求，使用`if(Val.trim()){...}`来请求

- 空对象使用null来定义，空数组用[]来定义，用.length来判断是否为空

### 路由

> 使用vue-router 

1. 安装： `npm install vue-router`

2. 创建路由：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
1. 定义
const routes=[
  {path:'/',component:PostList},
  {path:'/post',component:Post}
]
2. 实例化
const router = new VueRouter({ //可以添加路由模式
  routes,
  mode:'history' 	//默认为hash模式
})
export default router  //注意：导出的名字为router，不然在main.js中使用的时候需要用router:xxx
3. 挂载在main.js中
const app = new Vue({
  router
}).$mount('#app')
```

3. 使用路由：

- 展示路由页面需要`router-view`来展示使用<router-view></router-view>来展示

- 使用<router-link to="/foo">Go </router-link> 来实现跳转
  - `router-link`是包含匹配，可以加exact变为严格匹配
  - `active-class` 链接和地址栏匹配时候激活的样式
  - `exact-active-class` 链接被精准匹配时激活的样式

- 动态路由：`{path:'/post/:id',component:Post},`用冒号区分

- 获取地址栏信息 由于路由已经是全局配置了，所以用`this.$route`获取当前页面路由信息

- 路由默认的匹配是严格匹配，只能匹配一个，所以需要用到嵌套路由：

```html
to的内容也可以写成一个对象
<router-link :to="'home'">Home</router-link>  //普通写法
<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}"
  >Register</router-link
>
```

- 路由嵌套：子页面需要在父级页面对象内设置，使用`children`属性来设置，这是一个数组，里面包含对象，最后需要在父页面的页面结构中设置在哪里显示，使用`router-view`

- 404 ，使用*来匹配所有页面，
- 返回：` <button @click="$router.back()">back</button>` 同样，push也在$router中proto的push里

### ElementUI

1. 安装：`npm i element-ui`但是现在使用的是`vue add element`选择按需导入

2. 按需加载原理：在`bable.config.js`中配置了按需引入，在plugs文件夹下`element.js`中全局配置了。

3. 图形化界面安装 `vue ui`默认在8000端口打开，

4. 动画：

   - 动画的执行是异步的，要在刷新以后第一次执行动画，在mounted周期里面写上settimeout改成异步
   - 或者使用`setInterval`来生成。`setInterval`的使用：const一个变量等于setInterval函数，在函数里判断停止条件，使用`clearInterval(const的变量)`跳出计数器

5. 自定义主题：在线配置，然后下载zip包，解压以后是一个theme文件夹，放大src文件夹下，在

   如果是搭配 `babel-plugin-component` 一起使用，只需要修改 `.babelrc` 的配置，指定 `styleLibraryName` 路径为自定义主题相对于 `.babelrc` 的路径，注意要加 `~`。

   ```json
   {
     "plugins": [
       [
         "component",
         {
           "libraryName": "element-ui",
           "styleLibraryName": "~theme"
         }
       ]
     ]
   }
   ```

### 混入Mixin

全局：注册以后每个组件都使用

局部：需要的时候在导出中定义： mixin:[xxx]



指令传参

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。



注意：mixin并不是全局的，当导入mixin以后，里面的内容作用域仅在这个组件，修改了以后导入其他组件的内容并不会改变。



用来做登录认证

### 组件注册

在模块系统下（clil搭建的环境）：

````js
 name: 'HelloWorld',  //局部注册：
components: {   
    HelloWorld
  }
//在main.js中（也可以新建个js，专门用来注册）
Vue.component("name",被注册组件) //全局注册  name表示使用组件时的标签名
````



### 其他

移动端事件：

原生：`ontouchstart` `ontouchend` `ontouchmove` `ontouchcancle`

### vuex

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。

改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。

过程：

创建store：导入vue和vuex，然后`Vue.use(Vuex)`

最后在根结点注入store，把store的所有内容加入页面

```vue
const store = new Vue.Store({
	state:{
		count:0
},
	mutations:{   //提供修改state的方法
	add(state){
		state.count++
}
}
})
```

导出：` export default store`

在main.js中全局使用 store:store

在子组件中使用数据： this.$store.state

在子组件中使用方法： $store.commit('add')

`mapState`辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

```js
import { mapState } from 'vuex'  //导入

export default {
  computed: mapState({
    //1. 箭头函数可使代码更简练
    count: state => state.count,
    //2. 简写 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',
    //3/ 普通函数，为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

  当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个字符串数组。

```js
computed: mapState([     //写成数组（全都是第二种写法的简写）
  // 4. 	映射 this.count 为 store.state.count
  'count'
]) 
```

当本组件也需要用到加计算属性computed的时候，使用**对象展开运算符**，因为mapState函数返回的是一个对象

```js
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

#### Mutation

##### 提交载荷（Payload）

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
store.commit('increment', {
  amount: 10
})
```

##### 对象风格的提交方式

Mutation必须是同步的，异步不能写在mutation里面