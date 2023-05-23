# VUE理论基础

定义：VUE是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。采用的是。采用MVVM模式。

核心库和插件：视图层渲染、组件、路由（vue-router）、状态管理（vuex）

## MVVM模式

Model-View-ViewModel

<img src=".\image\Vue_MVVM模型.png" alt="Vue_MVVM模型" style="zoom: 50%;" />

代码中 层 的体现，见实例创建

> View 视图层：html中的标签（视觉上的东西）
>
> Model 数据层：Vue对象中的管理数据的属性（即data属性）
>
> viewModel 管理者：指Vue对象

------

## 引入方式

### vue2引入

> 1、直接CDN引入
>
> 方法：
>
> ```html
> <!--开发环境版本，包含了有帮助命令行和警告-->
> <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
> 
> <!--生产环境版本，删除了警告，优化了大小和速度-->
> <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
> ```

> 2、下载引入
>
>  方法：
>
> 开发环境版本，包含了有帮助命令行和警告： https://cdn.jsdelivr.net/npm/vue/dist/vue.js
>
> 生产环境版本，删除了警告，优化了大小和速度： https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.esm.browser.js

> 3、NPM安装
>
> 方法：使用vue-cli安装

### vue3引入

CDN

```html
<script src="https://unpkg.com/vue@next"></script>
```

------

## 实例创建

### Vue2创建

```html
<body>
    <!-- View 视图层 -->
    <div id="app">{{msg}}</div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        //viewModel 管理者
        let vm = new Vue({
            el: '#app', 		//elem元素，用于绑定dom(TagName、id和class都可以)
            //也可以是   el: document.querySelect('#app')
            name: 'one',		//给这个vm实例命名（可以是中文）
            //Model 数据层
            data: { 			//提供数据
                msg: 'hello Vue2'
            }
        });
        console.log(vm);
    </script>
</body>
```

或

```js
let vm = new Vue({});
vm.$mount('#app');
```

#### el

> 作用：指向View层，用于绑定页面元素的
>
> 语法：`el: 'TagName/id/class/document.querySelector('xx')'`
>
> 注：
>
> > 1、可以绑定id和class、类名等
> >
> > 2、只能绑定一个元素，若多个元素都是一个class，则只绑定第一个（解决：用一个大盒子将其包裹起来）
> >
> > 3、el只能在实例中使用（在创建构造器的时候，使用el会报错；因此el只能在new出来的实例中使用）
> >
> > 4、绑定元素的其他方法：
> >
> > ```js
> > let vm = new Vue({}).$mount('#app');
> > ```

#### data

> 作用：指向Model 数据层
>
> 注：在Vue2中，data是一个对象；但是在Vue3中，是一个函数，return 一个对象
>

### Vue3创建

```html
<body>
    <div id="app">{{msg}}</div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        let vm = Vue.createApp({
            data(){
                return {
                    msg: "hello Vue3"
                }
            }
        });
        let app = vm.mount('#app');	//挂载到根组件（元素）上
        console.log(vm);
    </script>
</body>
```

注：在Vue3中，data是一个组件，必须是函数，必须带return（原因：组件是可复用的）（返回的对象都是独立的，避免了数据污染）

------

## 响应式原理

响应式是指函数与数据的关联

> 1、被监控的函数：vue2的watcher，vue3的effect（全称是reactive effect，是一个内部函数）
> 都是监控某一个函数的执行
> 日常中会被监控的函数有：render、computed、watch、watchEffect
>
> 2、函数运行期间用到了响应式数据：ref、reactive
>
> 3、响应式数据变化会导致函数重新运行

> Vue2：es5中的  Object.defineProperty
>
> Vue3：es6中的  proxy

------

## 插值表达式

> 定义：直接在html标签内放入变量名
>
> 语法：`{{ name }}`
>
> 参数：name：变量名
>
> 注：每一个大括号里面只能包含一个表达式或变量（即不能在两个变量中加空格，但是可以用+拼接）
>
> 如：
>
> ```html
> <h2>{{msg + num}}</h2>		拼接输出
> <h3>{{arr[1]}}</h3>			输出数组
> <h4>{{arr.length}}</h4>
> <h5>{{obj.name}}</h5>
> <h6>{{三元表达式}}</h6>
> ```
>
> 注：
>
> > 1、null和undefined都不会输出
> >
> > 2、只支持表达式，不支持其他语句，如 `{{if(1) return msg}}`
> >
> > 3、优缺点：优点是常用，缺点是存在闪烁问题（可以用v-clock解决）

------

# Vue基本指令

## 通常指令

指带有v-属性的特殊属性。

在Vue给HTML元素增加了自定义属性，它们都是以 “v-” 开头了（原因：表明这是一个vue指令）。并且一些特殊的指令可以带参数和修饰符指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

三种将数据渲染到页面的方法：插值运算符、v-text、v-html

### v-text

> 语法：`<div v-text='msg'></div>`
>
> 参数：msg：一个变量，一般是vue实例中的属性
>
> 注：
>
> > 1、多个变量不能空格隔开，而是用+拼接
> >
> > 2、拼接字符串应该注意引号
> >
> > 3、在v-text标签中不能添加内容，会被覆盖（v-text的内容会覆盖标签原有内容）
>
> 优缺点：
>
> > 优点：无闪烁
> >
> > 缺点：覆盖标签原内容；无法渲染HTML格式

### v-html

类似innerHTML，即将字符串当成HTML渲染

> 语法：`<div v-html='msg'></div>		msg ='<p>我是HTML标签</p>'`	
>
> 参数：msg：一个变量，一般是vue实例中的属性
>
> 注：
>
> > 1、可以拼接字符串
> >
> > 2、可以使用三元运算符
>
> 优缺点：
>
> > 优点：无闪烁；可渲染
> >
> > 缺点：覆盖标签原内容

### v-clock

> 语法：
>
> ```html
> <style>
> 	[v-clock]{
> 		display
> 	}
> </style>
> <body>
> 	<div v-clock>{{msg}}</div>		//（不需要表达式，即无需 = ）
> </body>
> ```
>
> 作用：解决插值表达式闪烁问题

### v-once

> 语法：`<p v-once>{{msg}}</p>`
>
> 作用：只在初始化的时候插入一次值，当数据发生变化时，**不再发生更新**

### v-pre

> 作用：不编译指令，即让插值表达式等不执行

### v-model

> 语法：`<p v-modele='msg'></p>`
>
> 作用：针对表单元素和组件，实现**双向绑定数据**
>
> 只能绑定在表单元素上
>
> ```html
> <div class="app">
>      <input v-model="msg"></input>
>      <p>{{msg}}</p>
> </div>
> 
> <script>
>  let vm = new Vue({
>      data: {
>          msg: 'hello Vue'
>      }
>  }).$mount('.app')
> </script>
> ```
>
> v-model可以解决input标签中单选按钮必须加name的问题
>
> ```html
> <div class="app">
> 	<input type="radio" v-model="sex" value="男">男
> 	<input type="radio" v-model="sex" value="女">女
> 	<p>{{sex}}</p>
> </div>
> 
> <script>
> 	let vm = new Vue({
>         data: {
>         msg: 'hello Vue',
>         sex: '男'			//单选按钮会默认选择value和sex匹配的按钮（如果radio不给value，则点击按钮后，插值表达式 sex 会显示null）
> 		}
> 	}).$mount('.app')
> </script>
> ```

#### 修饰符

修饰符可以连续调用，如`<p v-modele.lazy.trim = 'msg'></p>`

##### lazy

> 语法：`<p v-modele.lazy = 'msg'></p>`
>
> 作用：不需要实例和标签实时发生改变时，需要加入lazy修饰符
> 即：当输入框内容变化时，vue实例的属性不发生变化；只有 敲回车 或 失去焦点 时，才更新实例

##### number

> 语法：`<p v-modele.number='msg'></p>`
>
> 作用：将输入的值，转换为数值类型（含浮点数）
>
> 注：
>
> > 1、一开始如果输入的是数字，就算后面是字母，也只输出数字（如：“123abc45”，在实例中是123）。类似于Number()
> >
> > 2、一开始不输入数字，则输出字符串（如：输入“abc123”，实例的data中是“abc123”）

##### trim

> 语法：`<p v-modele.trim='msg'></p>`
>
> 作用：去除首尾空格

### v-show

> 语法：`<p v-show="判断表达式">`
>
> 参数：判断表达式：可以为表达式，也可以为变量
>
> 作用：通过判断，是否显示该内容。如果值为true，则显示。否则就隐藏
>
> 如：
>
> ```html
> <p v-show="flag" class="app">{{msg}}</p>	//显示与否与flag真假有关
> <script>
> 	let vm = new Vue({
> 	    data: {
> 	        flag: '1'
> 	    }
> 	}).$mount('.app')
> </script>
> ```
>
> 注：不占页面位置，但是占用dom节点位置
>
> 切换频繁就用v-show

### v-if 

> 语法：`<p v-if="判断表达式" = 'msg'>`
>
> 作用：判断是否加载固定的内容，如果为真，就加载；否则不加载（既看不到元素节点，也看不到内容）
>
> 注：
>
> > v-else：与v-if搭配使用，原理与if-else一样（v-else需要紧跟v-if 或 v-else-if）
> >
> > v-else-if：`v-if='x'	v-else-if='xx'	v-else-if='xxx'	v-else='xxxx'`

### v-for

> 语法：`<li v-for="item in arr">{{item}}</li>`
>  		  `<li v-for="(item, index) in arr">{{item}}</li>`
>
> 参数：
>
> > item：数组的每一项
> >
> > index：下标
> >
> > in：in 和 of 都可以，没有区别
> >
> > arr：原数组
>
> 对于对象来说：`<li v-for="(item, key, index) in obj">{{item}}</li>`			item指属性值，key是指对象的属性（/方法）名
>
> 作用：将一个数组遍历或枚举一个对象循环显示，需要结合着 in 或 of 来使用

#### :key

> 语法：`:key="xx"`
>
> 参数：xx：一般为number、string
>
> 目的：使标签与key的值绑定，实现高效的更新虚拟DOM
>
> 使用：官方推荐在使用v-for时，给对应的元素或组件添加上一个:key属性
>
> 注：key的值不易与下标index相同，因为index会随着数组变化而变化

> 如果不加key，那对于节点的增加或删除，只要触发节点操作，就执行一次；操作多次，执行多次；加了key，因为key与id一样，具有唯一性，因此对于节点的操作只有一次（比如想删除第二个，其index为1，如果没有key，则删掉后其下一个又会变为index=1，导致该删除操作可再次执行）
> 
>即：无key，状态默认绑定的是位置（index 下标）
> 有key，状态根据key值绑定的内容（建议用id绑定，以达到唯一性）

### v-bind

v-bind和v-on都是事件指令

> 语法：`v-bind:属性名= "常量 或 变量名"`		（v-bind可以省略）
>
> 如：`<img :src="'./xxx.png'">`
>
> 作用：动态地绑定一个或多个属性，或者一个组件prop到表达式（父子通讯）
>
> 注：v-bind必须跟JS表达式

#### 对类名的控制 :class

> 语法：`<p :class="['one', 'two']">`						`<p :class="['one', {'two': flag}]">`	（表示flag控制two这个类是否生效）
> 			`<p :class="[myclass: true]">`		（myclass是一个变量， : true 表示生效该class）	
>
> 作用：实现JS对类名的添加和删除

#### 对css的增删 :style

与JS对CSS的控制类似

> 语法：`<p :style="{color: 'red', fontSize: '50px'}">`
>
> 如：
>
> ```html
> <p id="app" :style="[one,two]">数组样式</p>
> <script>
>  let vm = new Vue({
>      data: {
>          one: {
>              color: 'deeppink'
>          },
>          two: {
>              fontSize: '50px'
>          }
>      }
>  }).$mount('#app')
> </script>
> ```
>
> 注：
>
> > **驼峰命名**	
> >
> > 跟对象时，对象属性值必须是字符串

### v-on

> 语法：`v-on:事件名="方法名"`		||		`@ 事件名="方法名"`
>
> 作用：绑定一个事件或多个事件，事件函数的参数名代表了在方法对象methods中所定义的函数
>
> 注：在标签中添加`@dragstart.prevent`，可以防止img、a标签等被拖拽

如：

```html
<div id="app">
    <p>{{num}}</p>
    <button v-on:click="num++">自增</button>
    <button @click="Print()">打印</button>
    //没有传参的情况下，可以不写括号那种
    <button @click="Print">log</button>
</div>
<script>
    let vm = new Vue({
        data: {
            num: 0
        },
        methods: {
            Print: function() {
                console.log(this);			//指向vm实例（如果不是fn，而是箭头函数，则this指向vm的上级）
                console.log(this.num);		
            },
        	doSomething(){}
        }
    }).$mount('#app')
</script>
```

#### v-on 传参

> 1、不加括号绑定事件，第一个形参默认为事件对象（如点击事件等）
>
> 2、加了括号，如需要得到事件对象，可以使用 **$event**
>
> ```html
> <div id="app">
>     <button @click="Do">不加括号，默认获得事件对象</button>
>     <button @click="Print('wz',$event)">手动获得事件对象</button>
> </div>
> <script>
>     let vm = new Vue({
>         data: {
>             num: 0
>         },
>         methods: {
>             Do(e) {
>                 console.log(e);					//打印：事件对象
>             },
>             Print(str, event) {
>                 console.log('str:', str);			//打印：'wz'
>                 console.log(event);				//打印：事件对象
>             },
>         }
>     }).$mount('#app')
> </script>
> ```

#### 绑定多个事件

不能简写，必须用v-on

```html
<div id="app">
    <button v-on="{click:one, dblclick:two}">绑定多个事件</button>			//注意双击是 dblclick
</div>
<script>
    let vm = new Vue({
        methods: {
            one() {
                console.log('单击');
            },
            two() {
                console.log('双击');
            },
        }
    }).$mount('#app')
</script>
```

------

## 自定义指令

自定义指令尽量不用大写字母

### 全局指令

> 语法：`Vue.directive(id, definition)`
>
> 参数：
>
> > id：指令名称（一般自定义）		id在节点元素中，需要绑定才能生效（即：`<div v-id> xxx </div>`，其中id为直接取的名字，如v-focus等）
> >
> > definition：1、定义对象，其属性一般是提供的钩子函数（如bind、inserted等）
> > 				  2、定义函数，**对应钩子函数bind和update**
>
> 注：全局指令需要写在vue实例之前

如：加载完页面光标自动在第二个输入框

```html
<div id="app">
    姓名：<input type="text"> <br> 
    特长：<input type="text" v-focus>
</div>
<script>
    Vue.directive('focus', {
        inserted: (el) => {				//此处的el为绑定的input这个dom
            el.focus();
        }
    })
    let vm = new Vue({
        el: '#app',
        data: {}
    })
</script>
```

### 局部指令

> 语法：在Vue实例中加入directives属性	`directives:{ id: definition }`		（当definition为函数时， “ : ” 和 function 可以一起省略）
>
> 作用域：只作用在绑定的标签中

如：转化大写

```html
<div id="app">
    <h3 id="app" v-upper-text="msg"></h3>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg: 'i am a message'
        },
        directives: {
            'upper-text': (el, binding) => {
                el.innerHTML = binding.value.toUpperCase();
            }
        }
    })
</script>
```

## 修饰符

### 通用修饰符

通用修饰符：这些修饰符是所有事件都通用的

| **修饰符** | **描述**                                      |
| ---------- | --------------------------------------------- |
| .stop      | 阻止事件冒泡（调用 event.stopPropagation()）  |
| .prevent   | 阻止默认事件（调用 event.preventDefault()。） |
| .capture   | 添加事件捕获                                  |
| .self      | 只能由自身触发事件                            |
| .once      | 该事件只会触发一次                            |

### 按键修饰符

按键修饰符：这些修饰符是便捷一些按键操作的，比如要判断按键的键值的时候

| **修饰符** | **描述**     |
| ---------- | ------------ |
| .enter     | 回车         |
| .tab       | tab键        |
| .delete    | 删除”和“退格 |
| .esc       |              |
| .space     |              |
| .up        | ↑            |
| .down      | ↓            |
| .left      | ←            |
| .right     | →            |

如：

```html
<input @keyup.enter="Print($event)" id='app'>			//只有当键盘的enter抬起时才出发Print函数
<script>
    let vm = new Vue({
        methods: {
            Print(e) {
                console.log(e);					
                console.log(e.keyCode);
            },
        }
    }).$mount('#app')
</scripts>
```

------

# Vue的实例方法

## 数据

### Vue.set()

通过下标索引到的数组，并更改时，Vue实例得到更改，但是视图层并不发生改变

解决：使用replace方法更改数组	或	使用Vue.set()

> 语法：`Vue.set(target,index,value)`	（全局起作用）	||		`vm.$set(target,index,value)`		（只有vm实例起作用）
>
> 参数：
>
> > target：目标（需要更改的对象，一般为数组或对象）
> >
> > index：下标
> >
> > value：更改后的内容

如：

```js
let vm = new Vue({
    data: {
        arr = [1, 'a', 2]
    },
    methods: {
        Do() {
            this.$set(this.arr, 1, 'b');			//结果：arr = [1, 'b', 2]
        },
    }
}).$mount('#app')
```

------

# 钩子函数

## 生命周期

生命周期的定义：每个 Vue 实例在被创建时都要经过一系列的初始化过程，例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

![Vue_生命周期](.\image\Vue_生命周期.png)

```js
let vm = new Vue({
    data: {},
    methdos: {},
    beforeCreate() {},
    created() {},
    beforeMount() {},
    mounted() {},
    beforeUpdate() {},
    updated() {},
    beforeDestroy() {},
    destroyed() {}
}).$mount()
```

### 生命周期的钩子函数

生命周期里也有8个钩子函数：`beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed`

#### beforeCreate

> 表示 实例在完全被创建之前会执行的函数，data、methods都没有初始化
>
> 因此在beforeMount里调用data和methods都会报错
>
> 语法：`beforeMount() {}`

#### created

> 表示 data、methods已经初始化好了
>
> 但是实例还没有挂载，因此不能获取dom元素（如$refs）
>
> 语法：`created() {}`

#### beforeMount

> 表示 挂载前，template已经在内存中编译好了，但还没有挂载到页面上
>
> 语法：`beforeMount() {}`

#### mounted

> 表示 实例已经脱离了创建阶段，进入到了运行阶段
>
> 一般操作dom，最早需要在mounted中进行
>
> 语法：`mounted() {}`

#### beforeUpdate

> 表示 当数据发生更新时，页面的数据还未改变，但是data中的数据改变了
>
> 语法：`beforeUpdate() {}`

#### updated

> 表示 虚拟dom已经是最新的缓存，即页面已发生更新
>
> 语法：`updated() {}`

#### beforeDestroy

> 表示 实例已经进入销毁阶段，data和methods还保留着（用于页面销毁时保存一些数据），但是dom只存在于缓存中，没法调用
> **！！！亲测中，所有数据和方法都没了！！！**
>
> 语法：`beforeDestroy() {}`

#### destroyed

> 表示 组件已经完全被销毁，所有数据和方法都不可用
>
> 语法：`destroyed() {}`

## 指令对象的钩子函数

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

| **函数**         | **描述**                                                     |
| ---------------- | ------------------------------------------------------------ |
| bind             | 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。 |
| inserted         | 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。 |
| update           | 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 。 |
| componentUpdated | 指令所在组件的 VNode 及其子 VNode 全部更新后调用。           |
| unbind           | 只调用一次，指令与元素解绑时调用。                           |

指令钩子函数会被传入以下参数（有顺序）：

| **函数** | **描述**                                                     |
| -------- | ------------------------------------------------------------ |
| el       | 指令所绑定的元素，可以用来直接操作 DOM。                     |
| binding  | 包含指令相关的对象，包含以下这些属性：name（id名）、rawName（v-id名）、modifiers（一个对象，其属性可以查看id的修饰符）、expression（该id绑定的内容，即 = 后面的内容）、value（ = 后面如果是变量，则value是变量的值）、oldValue、arg |
| vnode    | vue 编译生成的虚拟节点。                                     |
| oldVnode | 上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。 |

如：

```html
<body>
    <input type="text" id="ipt" v-focus.trim="msg">
</body>
<script>
    Vue.directive('focus', {
            inserted: (el, binding) => {
                console.log(binding);		
            }
        })
        let vm = new Vue({
            el: '#ipt',
            data: {
                msg: 'i am a message'
            }
        })
</script>
```

打印结果：					![Vue_钩子函数_binding](.\image\Vue_钩子函数_binding.png)

------

# 过滤器、计算属性和侦听器

## 过滤器

> 作用：自定义一个过滤器，最终实现文本转化、数据类型转变等
>
> 作用地方：插值表达式	和	v-bind表达式
>
> 过滤器包含：
>
> > 全局过滤器：Vue.filter
> >
> > 局部过滤器：filters{}
>
> 注：利用methods方法也可以代替过滤器，但其语义化不明确（所有事情需要方法来处理），耦合性高

### 全局过滤器

> 语法：`Vue.filter(id, function)`
>
> 使用：`<div> {{ xxx | id }} </div>`

### 局部过滤器

> 语法：在Vue实例中使用，同局部指令
>
> 使用：同全局过滤器

如：

```html
<div id="app">
    <h3 id="app">猪肉价格：{{price | formatPrice("￥/kg")}}</h3>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            price: 158
        },
        filters: {
            formatPrice(price, unit) {
                return (price / 10).toFixed(2) + unit;
            }
        }
    })
</script>
```

注：

> 1、过滤器的函数中，必须return，否则输出undefined
>
> 2、过滤器是使用在插值表达式中，用 | 和变量隔开	（ | 叫做管道符）
>
> 3、过滤器函数可以传参，在标签中，变量默认为第一个参数，括号内跟的参数为形参第二个（详见上述例子中的 unit 形参）
> 即：如果只需要处理一个参数，则函数不需要加括号
>
> 4、多个过滤器函数，可以用多个 | 隔开（如：`<div> {{ msg | fn1 | fn2 }} </div>`）
> 注意后面的fn2可能会覆盖fn1

## 计算属性

语法：`computed:{}`

和方法的区别：方法每次使用都会重新调用一遍；但是计算属性利用缓存机制，可以实现多个重复方法只执行一次，只有当结果发生变化时才重新执行

如：

```html
<div id="app">
    <p>{{total}}</p>
    <p>{{total}}</p>
    <p>{{total}}</p>
    <p>{{total}}</p>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            num: 27,
            price: 5
        },
        computed: {
            total() {
                console.log('我只执行一次');
                return this.num * this.price + '￥';
            }
        }
    })
</script>
```

可以将上述例子理解为：先执行computed里的计算函数，再将结果传给data（data的属性名就是计算函数的名字），最后利用插值表达式展示data的值

结果：只打印一次 ‘我只执行一次’

注：total是计算属性，是一个属性，因此**在标签中不加括号**

> 计算属性默认有两个属性：getter和setter（getter默认都有，用于获取值，但不能修改值；setter用于设置/修改值）		因为其底层是Object.defineProperty
>
> 语法：
>
> ```js
> computed:{
> 	fullName:{
>         	get(){},
>         	set(val){}
>    	}
> }
> ```
>
> get：回调函数，必须跟return
>
> set：val这个形参是get返回的内容

## 侦听器

> 语法：`watch:{ 需要被侦听的数据 }`
>
> 作用：侦听指定数据变化，当发生变化时，就调用侦听器内的函数
>
> 如：
>
> ```js
> let vm = new Vue({
> 	el: '#app',
> 	data: {
> 	    num: 0
> 	},
> 	methods: {
> 	    add() { //数量+1
> 	        this.num++;
> 	    },
> 	    reduce() { //数量-1
> 	        this.num--;
> 	    }
> 	},
> 	watch: {	//侦听mun的变化
> 	    num(newVal, oldVal) {		//newVal：num变化后的值		odlVal：num变化前的值
> 	        doSomethings();
> 	    }
> 	}
> })
> 
> 或者
> vm.$watch('num', function(){ doSomethings(); }, {immediate:true} )	//immediate：立即侦听
> //最后一个{}表示选项对象，可以选择deep、immdiate等
> ```
>
> 注：侦听器和methods同级

### 监听引用类型

> 原因：因为引用类型的改变，指挥改变堆数据，指针不会变，因此普通侦听没有用
>
> 解决：deep

#### deep

> 语法：`deep: true`		默认为false
>
> 使用：
>
> ```js
> watch: {
>    	num: {
>    	    //handler：类似于基本类型的侦听的函数
>    	    handle(newVal, oldVal) {
>    	        doSomethings();
>    	    }
>    	},
> 	deep: true		//开启深度侦听
> }
> ```
>
> 作用：侦听对象的全部属性和方法
>
> **注**：如果不想只有当数据变化时才侦听，即想一开始就侦听，那么可以在deep属性前加 `immediate: true`

#### 打点侦听

> 语法：对象加引号并在后面加 `.属性名`
>
> 作用：侦听对象某一个属性
>
> 使用：
>
> ```js
> watch: {
>    	'num.totle': {
>    	    handler(newVal, oldVal) {
>    	        doSomethings();
>    	    }
>    	},
> }
> ```
>

### 取消侦听器

Vue本身的$watch方法就是取消侦听器的方法，因此可以直接在$watch的回调函数中调用$watch

即：`let unwatch = vm.$watch('data', function() { unwatch() } )`

因此，在监听的回调函数中，如果`doSomething()`代码写在unwatch()之前，则表示监听一次后取消监听；写在unwatch()之后，则不执行

------

# 组件

作用：组件可以扩展 HTML 元素，封装可重用的代码

父组件是会要等子组件加载成功的。如果`Child`组件一直没加载成功，那么父组件会一直等着他。

## 组件命名

可以使用 	`		-		小驼峰		大驼峰`		但是在标签中，大写的字母必须使用 - 隔开，并化为小写（在Vue中可以不考虑，因为会隐式转换，但是Vue有 - 时，必须用引号包裹）

## 创建 extend

创建构造器，又称为拓展（不常用）

> 定义：Vue构造器的拓展，调用Vue.extend创建的是一个组件构造器
>
> 语法：`Vue.extend({obj})`
>
> 参数：obj：对象，一般有template、data（注：**data此时必须为函数**，**且有return**，一般return一个对象）

创建构造器后，可以选择将其挂载到已有实例上，或者选择注册一个新的组件实例（注册见component）

如：

```ts
let myextend = Vue.extend({
	template: `<div>xx
				  <p>xxx</p>
			  </div>`
});
new myextend().$mount('#app');		//将myextend构造器挂载到id为app的标签上
```

### template

> 使用原因：组件没有挂载点，因此需要先有个template模板
>
> 语法：`template: '<div></div>'`
>
> 使用地方：
>
> > 1、组件：
> >
> > <img src=".\image\Vue_组件_template.png" alt="image-20220719182242008" style="zoom:67%;" />
> >
> > 2、template标签里（模板为保持唯一性，必须使用id）：
> >
> > <img src=".\image\Vue_组件_template_2.png" alt="image-20220719182522762" style="zoom: 67%;" />
> >
> > 3、script标签里（一定要加`type="text/x-template"`，让浏览器知道此标签是html标签，而非js）
> > 不常用
> >
> > <img src=".\image\Vue_组件_template_3.png" alt="image-20220719183103658" style="zoom:67%;" />
>
> 注：
>
> > 1、template**只有一个根节点**，即不能存在多个父级元素；因此若有多个标签元素，需要彼此嵌套
> >
> > 2、标签元素中是可以绑定Vue指令的

## 注册 component

> 作用：将构造器的拓展 注册为组件实例（即将extend注册为一个Vue实例）
>
> 语法：`Vue.component(自定义标签, myextend)`
>
> 参数：
>
> > 自定义标签：自己可以定义的标签名字（注意不能有大写）
> >
> > myextend：组件构造器，可以直接用一个obj来代替extend创建这一过程（实际开发直接使用另外的vue文件）

如：

```html
<mycpn></mycpn>
<mycpn></mycpn>
<mycpn></mycpn>
//可以有多个自定义标签，且都会被注册为实例并绑定
<script>
	let myextend = Vue.extend({
		template: `<div>xx
					  <p>xxx</p>
				  </div>`
	});
	Vue.component('mycpn', myextend)
</script>
```

### 全局组件

> 定义：调用Vue.component()注册组件时，组件的注册是全局的，这意味着该组件可以在任意Vue示例下使用
>
> 作用域：只要是挂载了的标签，组件绑定后都能正常使用（即：满足Vue实例、实例中存在绑定的自定义标签）

如：

```html
<div id="app">
    <my-cpn></my-cpn>
</div>
<script>
	 Vue.component('my-cpn', { //选项对象
	    template: `
				<div>
					<h3 v-for="val in name">汤圆：{{val}}</h3>
				</div>
			`,
	    data: function() {
	        return {
	            name: ["黑芝麻", "水果味"]
	        }
	    }
	})
	new Vue({
	    el: '#app' //挂载对象
	})
</script>
```

### 局部组件

放在Vue实例中

语法：`components: {'自定义标签/组件名称': {选项对象} }`

```js
new Vue({
	el: '#app', //挂载对象
    components: {
        'test': {
            template: `
					<div>
						<h3>汤圆</h3>
					</div>
					`
        }
    }
})
```

### `<component>`

component做元组件

> 作用：渲染组件，减少冗余代码（比如一个div里面，根据不同情况渲染不同组件，就需要把所有组件写上，用v-if-else来判断，代码冗余）
>
> 语法：`<component :is=""></component>`
>
> is属性：选择渲染的组件
>
> 如：
>
> ```vue
> <script setup>
> 	import Foo from './Foo.vue'
> 	import Bar from './Bar.vue'
> </script>
> 
> <template>
> 	<component :is="Math.random() > 0.5 ? Foo : Bar" />
> 	<!-->表示当随机数大于0.5时展示Foo组件，否则展示Bar</-->
> </template>
> ```
>
> 

## 组件嵌套

直接创建两个构造器/选项对象，然后在父的template中添加子，会报错（显示没有注册child子组件）

即：

```html
<div id="app">
    <my-father></my-father>
    <my-child></my-child>
</div>
<script>
    let child = {
        template: `
        		  <div>
        		      <h3>I am child</h3>
        		  </div>
        		  `
    }
    let father = {
        template: `<div style="border 2px solid red">
           			  <h3>I am father</h3>
           			  <my-child></my-child>					//报错，显示没有注册my-child子组件
       			  </div>
        		 `
    }
    new Vue({
        el: '#app',
        components: {
            "my-child": child,
            "my-father": father
        }
    })
</script>
```

解决：

在父构造器中局部注册一个子组件

```html
<div id="app">
    <my-father></my-father>
</div>
<script>
    let child = {
        template: `
        		 <div>
        		     <h3>I am child</h3>
        		 </div>
        		 `
    }

    let father = {
        template: `<div style="border: 2px solid red">
            		  <h3>I am father</h3>
            		  <my-child></my-child>
        		  </div>
        		  `,
        components: {
            'my-child': child
        }
    }
    new Vue({
        el: '#app',
        components: {
            "my-father": father
        }
    })
</script>
```

## 组件的data

组件中的data选项必须是一个函数，返回一个数据对象

原因：同vue2和vue3的data一样，都是避免数据污染

## 组件通讯

组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。如果在子组件中强行使用父组件的数据，就会报错

组件之间的通讯又叫做组件的传值

传值的原因：在开发过程中，数据从服务器请求可能是由大组件来完成，因此小组件的数据可能需要从大组件中获取

组件之间的关系：

<img src=".\image\Vue_组件_通讯_关系.png" alt="Vue_组件_通讯_关系" style="zoom:67%;" />

### 父子组件通讯



<img src=".\image\Vue_组件_通讯_父子传值.png" alt="image-20220719185517483" style="zoom:50%;" />

实际开发中，对于多个组件共用一组数据时，常用vuex来传值；但是只有一组组件，则用父子通讯

#### 父传子props

父传子：需要将父组件的数据绑定到子组件的标签中；子组件在创建的时候也需添加一个props来接受数据（props只可读，不可写）

props可以是一个数组，也可以是一个对象（对象的属性值，可以做属性限制，也能传递多个值，如下所示）

```html
<div id="app">
    <my-son :movies="msg"></my-son>
</div>

<template id="son">
    <div style="border: 1px solid red;">
        <h4>这是子组件</h4>
        <ul v-for="i in movies">
            <li>{{i}}</li>
        </ul>
        <h5>{{myMsg}}</h5>
    </div>
</template>

<script>
    //创建子组件
    let MySon = {
        template: '#son',
        //数组是放自定义属性名
        props: {
            movies: Array,
            myMsg: {						//父组件没有传值给myMsg，因此会调用default的值；若父组件传值了，则使用传值
                type: String,				 //定义数据类型
                default: '我是默认值'
            }
        }
    }
    //父组件实例
    let vm = new Vue({
        el: '#app',
        data: {
            msg: '钢铁侠'
        },
        //注册子组件
        components: {
            MySon
        }
    })
</script>
```

结果：![Vue_组件_父传子_props对象](.\image\Vue_组件_父传子_props对象.png)

报错原因：props属性值限制了数据类型（数据类型不要用引号包裹）
因此，可以一个属性限制多个数据类型：`movies: [String, Array]`

给子组件传值时，props中的属性也是一个对象，如movies、myMsg都是对象，其属性有：

> type：定义数据类型（可以定义多个，如上所示）
>
> default：默认值，父组件不传值时显示默认值
>
> required：数据是否传值，true为必传（不传就报错），false（默认值）为可传可不传
>
> 注：default 和 required 不能共用

##### 静态使用子组件属性

```html
<div id="app">
    <!-- 静态传值 给 movies -->
    <my-son movies="唐人街探案三,复仇者联盟"></my-son>
</div>
<template id="son">
    <div style="border: 1px solid red;">
        <h4>这是子组件</h4>
        <h4>电影名称：{{movies}}</h4>
    </div>
</template>
<script>
    //创建子组件
    let MySon = {
        template: '#son',
        //数组是放自定义属性名
        props: ['movies']
    }
    let vm = new Vue({
        el: '#app',
        //注册子组件
        components: {
            MySon
        }
    })
</script>
```

因为在标签中给movies写定了值，因此是静态的

##### 动态使用子组件属性

相较于静态，只多个v-bind，将父组件的data的属性，传递给子组件（父组件的data的属性改变，子组件也跟着改变，从而实现了动态）

```html
<div id="app">
    <!-- 动态传值 给 movies -->
    <my-son :movies="msg"></my-son>
</div>
<template id="son">
    <div style="border: 1px solid red;">
        <h4>这是子组件</h4>
        <h4>电影名称：{{movies}}</h4>
    </div>
</template>
<script>
    //创建子组件
    let MySon = {
        template: '#son',
        //数组是放自定义属性名
        props: ['movies']
    }
    let vm = new Vue({
        el: '#app',
        data: {
            msg: '钢铁侠'
        },
        //注册子组件
        components: {
            MySon
        }
    })
</script>
```

##### propsData

通过new创建子组件的时候给子组件传值（只传第一次）

如：

```html
<div id="app">
</div>

<template id="temp">
    <div style="border: 1px solid red;">
        <h4>propsData:{{msg}}</h4>
    </div>
</template>

<script>
    //创建子组件
    let myExt = Vue.extend({
        template: '#temp',
        props: ['msg']
    });
    //实例化对象
    new myExt({
        propsData: {
            'msg': '我是propsData传的数据'
        }
    }).$mount('#app')
</script>
```

##### ref 和 $refs

> ref：用html标签上，指向的是该标签的dom元素，在Vue实例中调用$refs即可使用
>
> 如：
>
> ```html
> <p ref='myp' id='app'></p>
> <script>
>     let vm = new Vue({
>         el: '#app',
>     });
>     vm.$refs.myp			//结果：打印显示 id为app 的 p 标签
> </script>
> ```

##### props 和 ref的区别

> props：强调数据，不能调用子组件的属性和方法
>
> ref：强调引用，可以调用子组件的属性和方法，但不能传递数据

#### 子传父

当子组件需要向父组件传递数据时，就要用到自定义事件了

自定义事件的流程：

> 在子组件中，通过$emit()来触发事件
>
> 在父组件中，通过v-on来监听子组件事件

##### $emit()

> 语法：`vm_son.$emit(method, msg)`			这个方法是写在子组件上的
>
> 参数：
>
> > method：发送这个事件的名称
> >
> > msg：需要传递的参数或内容

##### v-on

自定义一个methods方法，方法这个函数的形参就是子组件传递过来的参数或内容

例：

```html
<div id="app">
    <!-- 注意自定义事件 update不需要加括号，加了括号的一个形参必须是$event -->
    <my-son @get-msg="update($event)"></my-son>				//接受子组件通过$emit传值事件，并触发父组件update方法，以调用传值内容
    <h4>父组件：{{father_msg}}</h4>
</div>
<template id="son">
    <div style="border: 1px solid red;">
        <h3>{{msg}}</h3>
        <button @click="deliver()">传值</button>
    </div>
</template>

<script>
    let Son = {
        template: '#son',
        data() {
            return {
                msg: '我是子组件的内容'
            }
        },
        methods: {
            deliver() {
                this.$emit('get-msg', this.msg);		//子组件通过$emit发起传值
                console.log('传值成功');
            }
        }
    }
    
    let vm = new Vue({
        el: '#app',
        data: {
            father_msg: ''
        },
        methods: {
            update(val) {								//用来处理传值内容的父组件方法，
                console.log('接受传值成功');
                console.log(val);
                this.father_msg = val;
            }
        },
        components: {
            mySon: Son
        }
    })
</script>
```

##### 升级版 .sync

通过`.sync`修饰符，省去了父组件v-on绑定方法这一步骤

变化之处：子组件的`$emit`，需要增加一个update；父组件的v-on绑定方法改为`v-on:xx.sync`并绑定data的属性

```html
<div id="app">
    <!-- 通过v-on绑定get-msg事件（get-msg是自定义事件），并赋值为父组件的data：fMsg -->
    <my-son :get-msg.sync="fMsg"></my-son>
    <h4>子组件传过来的值：{{fMsg}}</h4>
</div>

<template id="MySon">
    <div>
        这是子组件
    <button @click="deliver()">发射</button>
    </div>
</template>

<script>
    let MySon = {
        template: '#MySon',
        data() {
            return {
                msg: '我是子组件的msg'
            }
        },
        methods: {
            deliver() {
                this.$emit('update:get-msg', this.msg);		//emit的第一个参数，必须跟update；get-msg是自定义事件，配合父组件的v-on使用
            }
        }
    }

    let vm = new Vue({
        el: '#app',
        data: {
            fMsg: ''
        },
        components: {
            MySon
        }
    })
</script>
```

### 非父子组件通讯

非父子组件的通讯主要是依靠事件车来实现

> 中央事件总线，又名eventBus事件车
>
> 实质：创建一个vue实例，通过一个空的vue实例作为桥梁实现vue组件间的通信

#### 事件车

> 1、在脚手架中，创建一个公共js文件，用来传递信息
>
> 2、引入vue.js
>
> 3、创建空的vue实例
>
> 4、向外暴露

eventBus.js：

```js
// 引入vue.js
import Vue from "vue";
// 创建空的Vue实例
let bus = new Vue();
//提供外部引入的接口
export default bus;
```

#### 触发事件

> 1、在需要传递数据的地方引入事件车，此处引入是eventBus.js下的bus
>
> 2、给bus绑定$emit，与子传父的类似，只不过是**`bus.$emit("事件名", "携带参数")`**
>
> 3、在需要接受数据的地方引入事件车，同时给bus绑定$on，即 **`bus.$on("事件名", function() {})`**	fn为回调函数
>
> 注：
>
> > 如果存在多个接收数据，则它们是同时接受的（即，可以实现广播）
> >
> > 一个事件车bus，可以存在多个$emit，他们互不干扰

传递数据：

```js
methods: {
	deliver() {
		console.log(bus);		//可以查看到bus是一个vue实例
		bus.$emit("deliverData", this.list);		//传递data里的list数据
	},
}
```

接收数据：

```js
mounted() {
	bus.$on("deliverData", (val) => {
		console.log(val);	//val就是传递的值
	});
}
```

#### 销毁

**`bus.$off("事件名")`**

------

# 插槽

## 使用原因

组件的插槽：让封装的组件更加具有扩展性

> 对于一个封装的组件，若要复用，肯定会有相同的和不同之处（比如底部导航栏，可能是首页+我的，也可能是购物车+设置）
>
> 解决：抽取共性，保留不同：
> 将共性抽取到组件中，将不同暴露为插槽。让使用者根据自己的需求，决定插槽中插入什么内容

## 基本使用

### 默认插槽 slot

在父组件中的子组件标签内写内容，会被覆盖（子组件的template模板会覆盖）

```html
<div id="app">
    <my-son>我是父组件</my-son>				//结果：“我是父组件”不会显示，而是显示子组件的template模板 的内容
</div>
```

但是如果子组件预留了插槽slot，则能够实现 内容 的显示

slot给了默认值，当父组件的子组件标签无内容，则显示默认值；有内容，则显示内容（多个内容也全部显示）

```html
<div id="app">
    <my-son></my-son>												<!--结果：先显示 “我是子组件” ， 再显示 “我是插槽”-->
    <my-son>我是父组件</my-son>										 <!--结果：先显示 “我是子组件” ， 再显示 “我是父组件”-->
    <my-son> <div>我是div</div> <span>我是span</span> </my-son>			 <!--结果：先显示 “我是子组件” ， 再显示 “我是div” “我是span”-->
</div>

<template id="my-son">
    <div>
    	<h1>我是子组件</h1>
    	<slot>我是插槽</slot>
    </div>
</template>
```

### 具名插槽

使用具名插槽之前，如果子组件有多个slot，那么父组件的内容将会吧子组件的slot全部替换，如：

```html
<div id="app">
    <my-son>我是父组件</my-son>										 //结果：显示三个 “我是父组件” 
</div>

<template id="my-son">
    <div>
    	<p><slot>我是插槽1</slot></p>
    	<p><slot>我是插槽2</slot></p>
    	<p><slot>我是插槽3</slot></p>
    </div>
</template>
```

给slot一个name标签，就可以解决全部替换的问题，如：

```html
<div id="app">
    <my-son>						
        <div v-slot:no1>								<!-->或者写成   <template #no1>  <-->
        	我是父组件
        </div>
    </my-son>										 //结果：显示 “我是父组件”  “我是插槽2”  “我是插槽3”
</div>

<template id="my-son">
    <div>
    	<p><slot name="no1">我是插槽1</slot></p>
    	<p><slot name="no2">我是插槽2</slot></p>
    	<p><slot name="no3">我是插槽3</slot></p>
    </div>
</template>
```

在slot标签中(创建slot时)，name也是一个属性，因此可以通过v-bind绑定，实现对name值的动态绑定

在使用slot时，也可以动态绑定：`v-slot:[name]`或 `#[name]`

注：

> 1、具名插槽在被调用时，只能用div或者template包裹起来
>
> 2、一般不用 `slot="xxx"`，而是使用 `v-slot:xxx`或者 `#xxx`
>
> 3、创建具名插槽时，不给name，则默认name='default'；在调用具名插槽时，不指定名字，默认是覆盖default插槽

### 作用域插槽 v-slot

插槽的内容无法访问到子组件的状态，因此使用作用域插槽解决：通过创建插槽时，给一个类似props

#### 编译作用域

父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。

如：

```html
<div id="app">
    <my-son></my-son>
    <my-son v-show="isShow"></my-son>								//子组件的isShow是false，但是页面仍然显示。因为父组件的isShow为true
</div>

<template id="mySon">
    <div style="border: 2px solid deeppink; margin: 20px 40px;">
        <h4>我是子组件</h4>
    </div>
</template>

<script>
    let mySon = {
        template: '#mySon',
        data() {
            return {
                isShow: false
            }
        }
    }
    let vm = new Vue({
        data: {
            isShow: true
        },
        components: {
            mySon
        }
    }).$mount('#app')
</script>
```

#### v-slot

> 作用：当数据需要在多个父组件多个界面展示的时候，将内容放在子组件插槽中，父组件只需要告诉子组件使用什么方式展示界面。
>
> 注：v-slot 只能用在 template 模板或 components 里面

```html
<div id="app">
    <my-son>
        <!-- 使用v-slot接受具名插槽abc的数据，并把它命名为cba（cba是随便自定义的） -->
        <template v-slot="cba">
            <!-- 在没有使用作用域插槽前，该template无法拿到子组件mySon的arr数据的 -->
            {{cba}}
        </template>
    </my-son>
</div>

<template id="mySon">
    <div style="border: 2px solid deeppink; margin: 20px 40px;">
        <h4>我是子组件</h4>
        <!-- 定义具名插槽abc，并且传入数据arr -->
        <slot :abc="arr">
            <ul v-for="i of arr">
                <li>{{i}}</li>
            </ul>
        </slot>
    </div>    
</template>

<script>
    let mySon = {
        template: '#mySon',
        data() {
            return {
                arr: ['no1', 'no2', 'no3']
            }
        }
    }
    let vm = new Vue({
        data: {},
        components: {
            mySon
        }
    }).$mount('#app')
</script>
```

结果：			{ "abc": [ "no1", "no2", "no3" ] }

返回的是一个对象，因此可以在插值表达式里面调用属性：cba.abc

因为和props很类似，因此其也能同时传递多个值，再用cba来打点调用

### 具名作用域插槽

```vue
<div id="app">
    <my-son>
    	<template v-slot:item="itemProps">我是插槽item，我接受的传值是 {{itemProps.count}}</template>
        <!-->v-slot跟的是slot的名字，等号之后的则是传过来的obj，可以直接obj打点调用传过来的内容<-->
    </my-son>
</div>

<template id="mySon">
    <div>
        <slot name="item" count="1"></slot>
    </div>
</template>
```

------

# 脚手架

## vue-cli3

### 创建

1、vue create 项目名

2、配置设置

3、启动项目：cd 项目名 	然后		npm run serve

### 使用

1、创建目录以及引用：在cli3项目中，main.js里面创建的vue实例一般只引用App，自己的vue文件等都是在components文件下下创建，然后再去App.vue里面引用（而非直接在main.js里面引用）

2、向外暴露：在自己的vue文件下的`<script>`标签里，利用`export default{}`，完成向外暴露。
`export default {}`：里面可以包含vue实例的所有属性，如props、name等

3、引入：在App.vue文件下的`<script>`标签里import，然后`export default { components:{xxx} }`，即可引入xxx子组件。

## 样式

`<style></style>`标签

其属性可以有

> lang：定义后缀，如：`lang="css"   lang="less"  lang="scss"`
>
> scope：给该vue组件下的样式一个单独的作用域（可以在浏览器元素中看到 `data-v-xxxxxx`，这个就是css唯一标识符的意思）
> 没有scope，组件样式会共享

### 深度选择器

在实际开发中，因为用到了UI组件，UI组件的样式是固定的，但是在自己定义的组件中样式又有scope，又想在此处改UI组件的样式，此时就需要用到深度选择器

> 语法：`>>>` 	或	`::v-deep`	或	`:deep(类名)`
> 后两者最常用
>
> 如：
>
> ```css
> ::v-deep .btn {
>     background: red;
> }
> 
> :deep(.btn) {
>     color: pink;
> }
> ```
>
> 

------

# VueRouter

本质：建立url和页面之间的映射关系

使用路由而不使用`<a>`的原因：vue是单页（一个html）复应用

解决路由向同一个路由跳转时（重复点击一个路由），报错：

> 方法1、安装老版本router
>
> npm i vue-router@3.0 -S
>
> 方法2、在main.js或router.js中添加
>
> ```js
> const originalPush = VueRouter.prototype.push;
> VueRouter.prototype.push = location => originalPush.call(this, location).catch(err => err)
> ```
>
> 

## 基本使用

1、安装插件：npm install vue-router --save

2、在模块化工程中使用它

原因：vue-router是一个插件

### 引入

vue-router常用的版本有v2和v4，v2是支持vue2，vue2以上的则是支持vue3的

#### v2版本

> (1) 导入路由对象，并且调用 Vue.use(VueRouter)
>
> ```js
> import Vue from 'vue' 
> import VueRouter from 'vue-router' 
> Vue.use(VueRouter)
> ```
>
> (2) 创建路由实例，并且传入路由映射配置，并在Vue实例中挂载创建的路由实例
>
> 此时需要在main.js文件中进行配置（或单独设置一个router.js文件，并在main.js只能够引入）
>
> ```js
> import Vue from 'vue';			//vue3是import { createApp } from 'vue';
> import App from './App.vue';
> import VueRouter from 'vue-router';
> //启动路由功能
> Vue.use(VueRouter);				//vue3是createApp(App).use(VueRouter);
> 
> //引用组件
> import Home from './View/Home.vue';
> import About from './View/About.vue';
> 
> 
> //1、配置路径与组件的映射关系（即路由路径和组件的对应关系）
> const routes = [{
>            path: '/home', //路由路径（即：网页的网址后缀）
>            component: Home //路由跳转的组件（即：网特跳转的页面）
>      },
>      {
>            path: '/about',
>            component: About
>      }
> ]
> 
> //2、创建路由实例，传入路由映射配置
> const router = new VueRouter({
>      routes, //传入路由配置
> });
> 
> new Vue({
>      router, //3、挂载路由
>      render: h => h(App),
> }).$mount('#app')
> ```
>
> **注**：routes是固定写法，不能改变（更换为任何其他单词都不可以）
>
> (3) 在App.vue文件中引入router-view标签，以展示 路由渲染的组件的页面
>
> ```vue
> <template>
>     <div id="app">
>          <h1>我是router-view外的内容</h1>
>          <!-- 利用router-view 展示 路由渲染的组件的页面 -->
>          <router-view></router-view>
>     </div>
> </template>
> ```
>
> 注：在router-view标签之外的标签，不会因为页面跳转而变化
> 即：router-view切换的是挂载的组件，其余内容不发生改变（常用在layouts不动的页面中）
> 因此，此处的h1标签会一直存在
>
> router-view是可以被router-link代替

#### v4版本

> (1) 导入路由对象

以下内容，v2和v4都通用

###  路由记录meta

meta是routes数组中的对象的一个属性，用于记录自定义信息（如访问权限等）

meta可以在路由地址和导航守卫上被访问到

> 语法：`meta: { xxx: xx }`
>
> 如：
>
> 在router.js中
>
> ```ts
> const routes = [{
>         path: '/home', 
>         component: Home,
>     	 meta: {
>             permission: 1,
>         }
>   },
>   	{
>    	     path: '/about',
>    	     component: About
>   	}
> ]
> ```
>
> 使用：如路由守卫
>
> ```ts
> router.beforeEach((to, from) => {
> 	to.meta.permission			//此时便可以访问meta里面的属性
> })
> ```
>
> 注：一个路由匹配成功后，它可能匹配多个路由记录（如匹配子路由，此时会成功匹配父路由和子路由的meta）

### useRoute()

> 作用：返回当前路由地址
>
> 语法：
>
> ```ts
> import { useRoute } from "vue-router"
> 
> useRoute();		//当前路由地址
> ```
>
> 注：必须在setup()中使用

### useRouter()

> 作用：返回router实例
>
> 语法：
>
> ```ts
> import { useRouter } from "vue-router"
> 
> useRouter();		//当前路由实例
> ```
>
> 注：必须在setup()中使用

### 声明式导航 router-link

该标签是一个vue-router中已经内置的组件, 它会被渲染成一个`<a>`标签

> 作用：展示 路由渲染的组件的页面
>
> 语法：`<router-link to="/xx"> </router-link>`		作用在App.vue文件下
>
> 参数：`to="/xx"`：表示跳转到地址为xx的页面上
>
> 如：
>
> ```vue
> <!-- 字符串 -->
> <router-link to="/home">Home</router-link>
> 
> <!-- 使用 v-bind 的 JS 表达式 -->
> <router-link :to="'/home'">Home</router-link>
> 
> <!-- 同上 -->
> <router-link :to="{ path: '/home' }">Home</router-link>
> 
> <!-- 命名的路由，跳转到name为user的路由上，并传值  `/user/123` -->
> <router-link :to="{ name: 'user', params: { userId: '123' }}">User</router-link>
> 
> <!-- 带查询参数，下面的结果为 `/register?plan=private` -->
> <router-link :to="{ path: '/register', query: { plan: 'private' }}">Register</router-link>
> 
> 
> <!-- 渲染结果 -->
> <a href="/home">Home</a>
> ```
>
> 注：
>
> > 1、router-link和router-view一样，必须使用在App.vue文件里面
> >
> > 2、每次切换页面，被渲染成的`<a>`进入的页面就会追加一个类名，其余则去掉这个类名

#### active-class

active-class是router-link的一个属性

> 作用：正常的**class类名将无法使用**，需要用`active-class=""`来添加自定义类名，最后就可以在style中使用自定义的类名了
>
> 语法：`<router-link to="/xx" active-class="自定义类名"> </router-link>`			作用在App.vue文件下
>
> 缺点：active-class不适合多个标签同时绑定一个类名（原因：代码冗余）
>
> 解决：在需要添加自定义类名的组件中的router实例中添加`linkActiveClass`

#### linkActiveClass

linkActiveClass是VueRouter实例的一个属性

> 作用：添加自定义类名
>
> 语法：作用在main.js或router.js下
>
> ```js
> const router = new VueRouter({
>        routes,
>        linkActiveClass: "自定义类名" //此时就给routes数组中的所有组件追加了 自定义类名
> });
> ```

#### tag

tag是router-link的一个属性

> 作用：正常的router-link会被渲染成`<a>`标签，加入tag后可以渲染成自定义标签
>
> 语法：`<router-link to="/xx" tag="标签"> </router-link>`		作用在App.vue文件下
>
> 参数：标签：可以是`button、li、span、p`等
>
> 注：
>
> > 1、不管渲染成什么标签，类名只能通过active-class或linkActiveClass方式添加
> >
> > 2、会留下路由history记录（浏览器可以通过history实现页面的前进后退操作）

#### replace

replace是router-link的一个属性

> 作用：自动删除该标签的history记录
>
> 语法：`<router-link to="/xx" replace> </router-link>`				作用在App.vue文件下
>
> 注：加了replace的标签才无history，没有加的仍然可以使用前进后退

## 编程式导航

router-link可以实现路由的快速跳转，但需要使用tag属性才能将`<a>`标签转化为其他标签

如果使用编程式导航，则可以输入任何html标签

### push跳转

> 语法：
>
> ```vue
> <template>
>  <div id="app">
>      <button @click="toHome">跳转</button>
>  </div>
> </template>
> 
> <script>
>  export default{
>      methods:{
>          toHome() {
>              this.$router.push('/home');
>          }
>      }  	    
>  }
> </script>
> ```
>
> 相当于：
>
> ```vue
> <router-link to="/home" tag="button">跳转</router-link>
> ```
>
> 因为push中的参数，可以使用变量的拼接，因此可以很好的实现动态路由（如：`this.$router.push('/home/' + user1)`）
>
> 注：
>
> > push跳转的页面是有缓存的，同时会保存历史记录
> >
> > push括号内的内容，不仅支持字符串，也支持对象

### replace跳转

语法和push一模一样

push和replace的区别

> push：每次跳转会增加一个history记录
>
> replace：不增加历史记录

## 路由的默认路径

> redirect使用原因：在路由使用中，默认打开的网页是 / 这个根路径，如果需要更改默认打开路径，需要使用redirect进行配置
>
> 作用域：routes这个路由配置的一个属性
>
> 语法：作用在main.js或router.js文件下
>
> ```js
> const routes = [{
> 	path: '/',
> 	redirect: '/home'
> }]
> ```
>
> 注：
>
> > path可以为 * 		表示除了被注册的路径外，输入其他人和路径，都会被重定向到redirect的网页
> >
> > 重定向，必须单独为一个对象，不能合着一个路由组件一起写（即，重定向无component）

## 路由模式

路由模式有两种：hash和history

hash：在默认路由使用中，浏览器url自带一个`/#`，表示这是一个哈希路由，表示有历史记录，且页面跳转不会造成页面刷新

history：url不带`/#`，且不保存历史记录，页面跳转也不会造成页面刷新

### 路由模式的改变

> 语法：
>
> ```js
> const router = new VueRouter({
> 	routes, 
> 	mode: 'hash'
> });
> ```
>
> 注：mode的属性值必须是**字符串**类型

## 动态路由

> 定义：路由（path）和组件（component）存在匹配关系，此时称之为动态路由
>
> 实际现象：浏览器的url中，可能因为用户名不同而不同（如github的网址）
>
> 作用：动态路由是一种传递数据的方式

### 传参

params和name搭配使用，query和path搭配使用

#### params传参

语法：在path最后加 `/:xx`	（如 `/:id`）

##### 方法一

to后面跟string

> ```js
>const routes = [{
> 	path: '/:xx',
> }]
> ```
> 
> 参数：xx：自定义参数，在网页的path路径下输入任何信息，都能使 xx=“输入的信息”
>
> 如：（**例子中含params传参**）
>
> > App.vue
>>
> > ```vue
> > <template>
> > 	<div id="app">
> > 	<router-link to="/home" tag="button">主页</router-link>
> > 	<router-link to="/about" tag="button">关于</router-link>
> > 	<!-- 通过v-bind绑定data中的数据 -->
> > 	<router-link :to="'/about/' + user1" tag="button">王泽</router-link>
> > 	<router-link :to="'/about/' + user2" tag="button">max</router-link>
> > 	<router-view></router-view>
> > 	</div>
> > </template>
> > 
> > <script>
> > export default {
> >         name: "App",
> >         components: {},
> >         data() {
> >             return {
> >                 user1: "王泽",
> >                 user2: "max"
> >             };
> >         }
> > };
> > </script>
> > ```
> >
> > index.js  或  router.js
> >
> > ```js
> > const routes = [{
> > 	path: '/:name',
> > }]
> > ```
> >
> > AboutView.vue
> >
> > ```vue
> > <template>
> >    	<div id="about">
> >    	    <h1>This is an about page</h1>
> >    	    <p>{{ uname }}</p>
> >    	    <p>{{ $route.params.name }}</p>//模板直接将vue进行托管了，因此不需要this
> >    	</div>
> > </template>
> > 
> > <script>
> > export default {
> >     computed: {
> >         uname() {
> >             //调用计算属性传值，也可设data进行传值
> >             return this.$route.params.name;
> >         }
> >     }
> > };
> > </script>
> > ```
> >
> > 结果：
> >
> > ![Vue_动态路由_例子](.\image\Vue_动态路由_例子.png)
> 

##### 方法二

to后面跟对象

> to的另外使用方法：跟对象 `<router-link :to="{}">`	（以下name均指data中的属性）
>
> > `{name:'USER', params: {id: name}}`		name表示该router-link的一个名字，params则是实现路由跳转，同样能够实现动态路径拼接
> > id：是指routes中 `/:id`
> >
> > 注：不写path，默认path就是 / 根路径
>
> 此外，push和replace也通用，括号内的参数可以是对象，方法同router-link中to跟对象一样
> 如：` this.$router.push({name:'XX', params: {id: 'xxx'}})`

#### query传参

> 也需要to跟对象 `<router-link :to="{}">`
>
> > `{path:'/XX', query: {id: 11, sex: 'male'}}`			此时浏览器url将会为：`http://localhost:8080/XX?id=11&sex=male`
>
> 此外，push和replace也通用，括号内的参数可以是对象，方法同router-link中to跟对象一样
> 如：` this.$router.push({name:'XX', params: {id: 'xxx'}})`

## 嵌套路由/子路由

使用嵌套路由，需要用children属性

> 语法：	写在routes里面
>
> ```js
> const routes= [
>     {
>         path: '/home',
>         component: Home,
>         children: [
> 			{
> 				path: '/home/news',			//如果要加 “/” ，则必须把路径补充完整（不能只写 “/news” ，否则会被当成一个根路径来识别）
> 				component: News
> 			},
> 			{
> 				path: 'msg',				//或者不加 “/” 
> 				component: Msg
> 			}
> 		]
>     },
>     {
>         path: '/about',
>         component: About
>     }
> ]
> ```
>
> 此时就能访问 /home/news 路由以及 /home/msg 路由
>
> 注：**使用router-link标签，to属性中也需要注意 “/” ，重定向redirect也一样，需补完**
> 			**必须写成 `<router-link to="/home/news">`**

## 路由守卫

作用：用来监听监听路由的进入和离开的

vue-router提供了beforeEach和afterEach的钩子函数, 它们会在路由即将改变前和改变后触发

beforeEach作用：拦截、

afterEach作用：页面跳转后的提示、欢迎等

参数：

> to：目标导航的路由信息对象（即目标路由）
>
> from：离开的路由信息对象
>
> next：一个方法，是否要进入导航，如果需要进入导航就执行 next()		对于全局路由守卫，必须有next()，否则路由不会进行跳转
>          next的括号可以跟参数，一般跟跳转的页面路径 如：`next('/home')`

### 在全局路由中使用路由守卫

> 语法：
>
> > `router.beforeEach((to, from, next) => {})`
> >
> > `router.afterEach((to, from) => {})`			无next

### 独享守卫

> 定义：当跳转到指定路由时，触发的守卫
>
> 只有beforeEach
>
> 语法：写在routes里面  
>
> ```js
> const routes = [
> 	{
>         path: '/home',
>         name: 'home',
>         component: Home,
>         beforeEach(to, from, next) {}
>     }
> ]
> ```

### 组件内的守卫

又称组件内的钩子函数

beforeRouteEnter()		表示组件实例还未创建，因此this无法指向该实例

beforeRouteLeave()	   表示离开组件时触发。此时实例已注册完毕

beforeRouteUpdate	   表示该路由的路由信息变化后触发（如子路由的切换）		实例已注册完毕

------

# Axios

Axios是一个基于promise的HTTP库，可以用在浏览器和node.js中

特点：

> 1、支持从浏览器中创建XMLHttpRequests
>
> 2、支持从node.js中创建http请求
>
> 3、支持Promise
>
> 4、能够拦截请求和响应
>
> 5、转换请求数据和响应数据

## 使用

> 安装
> 引入
> 发起请求

### 安装

npm i axios

### 引入

全局引入

> 先在main.js中：import axios from 'axios'
>
> 然后挂载到vue原型上：Vue.prototype.$http = axios

### 发起请求

默认情况下，axios将JavaScript对象序列化为JSON。 要以`application/x-www-form-urlencoded`格式发送数据

#### 普通请求

##### get请求

```js
axios.get('http://xxxx')				//可以写成 axion('hettp://xxx')		因为axios默认是get请求
	.then((res) => {})
	.catch((err) => {})
```

注：res中的data属性才是获取的数据，res对象中还包括status等

##### post请求

```js
axios.post('http://xxxx', data)		
	.then((res) => {})
	.catch((err) => {})
```

data：需要传输的参数，一般是obj或str格式

#### 指定请求参数请求

方法一：在url上更改

```js
axios.get('http://xxx?userId=3')			//此时就可以获得userId为3的数据				
	.then((res) => {})
	.catch((err) => {})
```

方法二：指定params参数

```ts
axios.get('http://xxx', {params: {userId: 3}})						
	.then((res) => {})
	.catch((err) => {})
```

## axios的二次封装 axios.create()

> 作用：针对请求方法一样，地址不一样的axios，减少重复代码
>
> 语法：
>
> ```ts
> const instance = axios.create({
>    baseURL:"http://localhost:3000"
> })
> 
> // 使用instance发请求，默认是get请求
> instance({
>     url:"/posts"
> })
> 
> // 或
> instance.get("/posts")
> //此时访问的地址就是 http://localhost:3000/posts
> ```
>
> 同时请求多个端口：
>
> ```ts
> const instance = axios.create({
>     baseURL:"http://localhost:3000"
> })
> 
> const instance2 = axios.create({
>     baseURL:"http://localhost:4000"
> })
> 
> // 同时请求 端口号 3000 4000
> 
> // 使用instance发请求
> instance({
>     url:"/posts"
> })
> 
> // 使用instance2发请求
> instance2({
>     url:"/posts"
> })
> ```
>
> 

## 拦截器

作用：在请求或响应被 `then` 或 `catch` 处理前拦截它们

### 请求拦截器

> 语法：
>
> ```ts
> // 添加请求拦截器
> axios.interceptors.request.use(function (config) {	//config：请求配置
> 	// 在发送请求之前做些什么
> 	return config;
> }, function (error) {
> 	// 对请求错误做些什么
> 	return Promise.reject(error);
> });
> ```
>
> 使用场景：发送网络请求时，在界面显示一个请求的同步动画、某些请求（比如登录（token））必须携带一些特殊的信息

### 响应拦截器

> 语法：
>
> ```ts
> axios.interceptors.response.use(function (response) {	//response：响应结果
> 	// 对响应数据做点什么
> 	return response;
> }, function (error) {
> 	// 对响应错误做点什么
> 	return Promise.reject(error);
> });
> ```
>
> 

例子：

```ts
axios.interceptors.request.use(function (config) {
    console.log("请求拦截器")
	return config;
});

axios.interceptors.response.use(function (response) {
    console.log("响应拦截器")
	return response;
});

console.log("开始请求")
axios.get("http://localhost:3000/posts")
.then(res=>{
    console.log("res:",res)
    console.log("请求结束")
})
```

结果：

![Vue_axios_拦截器_results](.\image\Vue_axios_拦截器_results.png)

注：多个拦截器拦截一个请求时，请求拦截器 **后添加先执行**

------

# Vuex

定义：Vuex是实现组件全局状态（数据）管理的一种机制，可以方便实现组件之间的数据共享

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着应用中大部分的**状态 (state)**

Vuex 和单纯的全局对象有以下两点不同：

> 1、Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
>
> 2、**不能直接改变** store 中的状态。改变 store 中的状态的唯一途径就是提交 (commit) mutation**
> 如果直接调用$store.state进行修改，数据在页面显示的内容会变，但是devtools没法监听到（实际上数据是变了的）

注：在Vuex中，state是状态，也称为数据

## 使用

### 安装

npm i Vuex

### 引入

一般在脚手架中会新建store文件夹，并在里面创建index.js文件

在index.js中

```js
import Vuex from 'vuex';	 	 //引入			此时Vuex下并没有任何方法和属性

Vue.use(Vuex);					//挂载到Vue上

const store = new Vuex.Store({	 //创建实例方法
    state: {},					//存储状态
    mutations: {},
    getters: {},
    actions: {},
    modules: {}
});

export default store			//导出
```

| 属性      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| state     | 包含了store中存储的各个状态（里面可以定义自己需要共享的数据）<br />如：`state: {id:1, sex:'男'}` |
| getters   | 类似于 Vue 中的计算属性，根据其他 getter 或 state 计算返回值 |
| mutations | 一组方法，是改变store中状态的执行者，只能是同步操作（页面变化，devtools也发生变化；异步任务的话，devtools无法侦听到） |
| actions   | 一组方法，其中可以包含异步操作                               |
| moudules  | Module是store分割的模块，每个模块拥有自己的state、getters、mutations、actions |

在main.js中

```js
new Vue({
	//注入vuex
    store,
})
```

### 组件使用共享数据

#### 显示

$store.state.xxx		xxx为共享的数据名

#### 修改 commit

> 在store中：
>
> ```js
> const store = new Vuex.Store({				//this指向store实例对象
> 	state: {
>      		num: 0
> 	},
> 	mutations: {
>     		fn(state, payload) {}						//此时的state形参就是state对象，避免了this的使用；payload就是携带的参数
> 	}
> })
> ```
>
> 在methods中：
>
> > 字符串风格
> >
> > ```js
> > methods: {
> > 	Fn() {
> >         	this.$store.commit('fn', payload)		//字符串 fn 对应 mutations 中的 fn 函数；且传payload参数给mutations
> >    	}
> > }
> > ```
>
> > 对象风格
> >
> > ```js
> > store.commit({
> > 	type: 'fn',					//type的 fn 对应 mutations 中的 fn 函数
> >     变量名: 变量值				//在mutations中的payload得到的是这个对象，要使用变量值，则需要 payload.变量名
> > })
> > ```
> >
> > 整个对象都作为载荷（payload）传给 mutation 函数

注：如果state是一个引用类型，如果直接给其追加或修改属性，页面是不会更新的，可以利用深拷贝，将原有的复制一遍并重新传给state

如：

```js
const store = new Vuex.Store({
	state: {
		obj: {
            id: 1,
            sex: 'male'
        }
	},
	mutations: {
		fn(state, payload) {
			state.obj = { ...state.obj , name: 'xx'}			//深拷贝了obj，并追加了name属性
             //或者	Vue.set(state.obj, name: 'xx')
		}						
	}
})
```

## actions

和mutations差不多，与mutations的不同点：

> 1、不过actions可以包含异步操作，而mutations只能包含同步操作
>
> 2、action 提交的是 mutation，而不是直接变更状态

> 语法：
>
> > 在store中：
> >
> > ```js
> > actions: {
> > 	fn (context, payload) {
> >    		context.commit('mutations内的函数名', 参数);		//本操作就是：actions先提交mutation，mutation再去改变state数据
> >    	}
> > }
> > ```
>
> 在methods中：
>
> ```js
> store.dispatch('fn')
> ```

异步的代码可以放在actions中，然后用dispatch去触发actions

如：

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

## getters

可以认为是 store 的计算属性，getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

> 语法：
>
> 在store中：
>
> ```js
> getters: {
> 	fn(state, getters) {},
> 	fn1() {}
> }
> ```
>
> 这样就能在fn函数内调用getters的其他函数，如 `fn(state, getters) { getters.fn1 }`
>
> 在methods中：
>
> ```js
> store.getters.fn				 //如果fn不带形参 或 只有 state 形参，则可以省略括号
> ```

> 当methods中的`store.getters.fn(a)`需要传a过来时，需要将getters的fn改为：
>
> ```js
> getters: {
> 	fn: (state) => (形参) => {}			//此时形参接收到的实参就是 a 
> }
> 
> //或者
> 
> getters: {
>     fn(state, getters) {
>         return (形参) => {}				
>     }
> }
> ```
>
> 

注：在getters声明的函数，必须有return

------

# Pinia

类似于Vuex，是专为vue框架开发的（可以理解为Vuex 5）

Pinia和Vuex的区别：

> 1、没有mutations：Pinia的actions支持同步和异步
>
> 2、无需创建各个模块：在Vuex中，如果数据过多，是需要分模块来管理；而Pinia中每个store都是独立的，互不影响

## 安装

npm install pinia

## 引用

在main.js中：

```ts
import { createPinia } from 'pinia'
const pinia = createPinia();
const App = createApp(App);
App.use(pinia)
```

## store

### 创建

> store作用：存储数据，其他人可以随便访问和修改里面的数据
>
> 语法：在store文件夹下
>
> ```ts
> import { defineStore } from "pinia"
> 
> export const useXxxStore = defineStore(id, options)		//规范：use开头，Xxx自定义名字，Store结尾
> ```
>
> 参数：
>
> > id：自定义store名字（注意其唯一性）
> >
> > options：一个对象，是store的配置项，用于配置数据、修改数据的方法等

### 引用

```ts
import { useXxxStore } from './store/xx.ts'
const xxxStore = useXxxStore();
```

### options下的属性

#### state

> 定义：defineStore中第二个形参{}下的一个属性
>
> 作用：存储数据
>
> 语法：`state: ()=> { return {} }`

##### 使用

直接打点调用：`xxxStore.xx`

因为state中存储的数据是直接挂载在xxxStore对象下的，因此也可以使用解构赋值来解构xxxStore对象
注：**不过需要注意使用storeToRefs()来将其变为响应式：`const {a, b, c} = storeToRefs(xxxStore)`**		用toRefs也可以（ 见Vue3 —> 组合API —> toRefs() ）

##### 修改

不需要像Vuex一样利用commit去修改，直接赋值即可：`xxxStore.xx = newVal`

##### 重置$reset()

> 作用：清楚state数据被变化的部分，从而恢复到state原有赋值的情况
>
> 语法：`xxxStore.$reset()`

##### 批量修改$patch

> 作用：一次性修改多个state中的数据，避免了多次打点调用
>
> 语法：`xxxStore.$patch({})`
>
> 如：
>
> ```ts
> storeToRefs.$patch({
>     name: 'wz',
>     age: 18
> })
> 
> //此时state中的name和age就会改变
> ```

##### 替换$state

> 语法：`xxxStore.$state({})`

#### getters

同Vuex的getters，算pinia的一个计算属性

> 语法：`getters: { 方法 }`
>
> 如：
>
> ```ts
> defineStore('test', {
> 	state: () => {
>         name: 'wz',
>         age: 18
>     },
>     getters: {
>         addAge(state) {					//形参第一个必须是state
>             return state.age++;		 	 //必须有return
>         },
>         addName() {						//用this，不能用箭头函数
>             return this.name + '!';		 //同样能拿到state的name，此时的this指向store实例xxxStore
>         },
>         add(state) {
>             return num => state.age + num;		//返回了一个函数，实现了向getters中的add函数传实参
>         }
>     }
> })
> 
> //使用getters
> {{xxxStore.addAge}}
> {{xxxStore.add(1100)}}		//向getters中的add函数传实参1100
> ```
>

#### actions

与Vue2的methods类似，用于放置一些逻辑处理的代码，该方法可以是同步也可以是异步

actions也是一个对象

> 使用场景：发送请求等
>
> 语法：`actions: { fn() {} }`
>
> 注：fn不带state形参，因此修改state的内容需要用到this（this指向store实例）
>
> 使用：直接 xxxStore.fn 即可调用

------

# Vue3

与Vue2对比的特点：Vue3中设计了一套强大的composition组合APi代替了Vue2中的option API ,复用性更强了，对TypeScript有更好的支持

区别（只有部分）：

> 1、创建：**Vue3直接使用createApp创建**；Vue2则需要new一个Vue实例，再利用render渲染
>
> ```ts
> import { createApp } from 'vue';
> createApp().use();
> ```
>
> 2、在vue文件中，舍弃了propsData的父子通讯，采用defineProps进行自定义props属性
>   reactive和ref：在setup中返回的数据不是响应式的，必须使用reactive或ref函数来包装，才能进行引用

vite创建：npm init vite 自定义名称

## 组合API

Composition API就是组合API，是Vue3.x的新特性

作用：提供了代码的共享，提高了代码的可读性（虽然Vue2的options API也能实现代码共享，但是其只能在特定的区域使用，比如数据只能在data中定义）

### setup()

> 定义：是组合API的入口函数			（computed、watch、methods等都是定义在setup中）
>
> 语法：`setup(props,context) {}`
>
> 参数：
>
> > props：组件传入的属性，指的是props对象（props对象类似Vue2的props）
> >
> > context：上下文对象			包含emit（对应Vue2的$emit）属性、slots插槽属性、attrs
>
> 注：在setup中不能用this访问到Vue实例了，只能通过context来访问Vue实例中最常用的属性

setup中若需将 变量或方法 传给 Vue实例或插值表达式 （类似将数据写在data中），需要将其return出去	如：`setup() { let num=0; return {num} }`
但是此时的 变量或方法 并非响应式的数据（即：无法实现双向绑定 v-model）
解决：利用ref 或 reactive

注：

> 1、在Vue3.2之后，setup函数写在 `<script setup></script>`里面了，同时不需要return，但同样需要ref和reactive。
> 如果需要异步，则在script顶部加上await表达式
>
> ```vue
> <script setup>
> const res = await fetch(...)
> const posts = await res.json()		//此时的posts就是异步等待后得到的
> </script>
> 
> <template>
>   {{ posts }}
> </template>
> ```
>
> 2、以下所有都是在setup函数里面，如 ref ~ watch、生命周期

### ref()

> 作用：ref()函数接受一个数据，返回一个响应式的数据对象
>
> 引入：`import { ref } from "vue"`
>
> 语法：`ref(值)`
>
> 在setup中的双向绑定：
>
> ```js
> setup(){
> 	let num = ref(0);			//此时的num就可以双向绑定了
>    	console.log(num);			//此时num成了一个对象，value属性是num的值
> 	return { num }
> }
> ```
>
> 注：ref括号内只能有一个值，对于多个值，需要用数组包裹
>
> 在setup中，对于数组值的获取：
>
> ```js
> setup(){
> 	let arr = ref(['abc', 123, 'boy']);			
>    	console.log(arr.value[0])			//是arr.value[index]，而非arr[index]
> 	return { arr }
> }
> ```
>
> 注：如果将对象分配为 ref 值，则可以通过 reactive 方法使该对象具有高度的响应式

### reactive()

> 作用：reactive() 函数接收一个普通对象，返回一个响应式的数据对象
>
> 引用：`import { reactive } from "vue"`
>
> 语法：`reactive(obj)`
>
> 注：
>
> > 1、通过reactive生成的，不需要像ref一样通过value属性来获得值，而是其本身就是对象数据
> >
> > ```js
> > setup(){
> > 	let obj = reactive({name: 'wz'});
> >    	console.log(obj.name);				//结果：'wz'
> >    	return { obj }
> > }
> > ```
> >
> > 2、对于obj对象中包含引用数据类型，若不想连续打点调用，可以使用toRefs
> >
> > ```js
> > setup(){
> > 	let obj = reactive({
> >         person: {
> >          name: 'wz',
> >             age: 18
> >         }
> >     });
> >     return { ...obj }			//将对象拓展后返回			缺点：person对象并  ！非响应式数据 ！
> > }
> > 
> > 外界引用只需person.name
> > ```

### toRefs()

> 作用：把响应式的对象转化成普通对象
>
> 引用：`import { toRefs } from "vue"`
>
> 语法：`toRefs(obj)`
>
> ```js
> setup(){
> 	let obj = reactive({
>         person: {
> 		   name: 'wz',
>             age: 18
>         }
>     });
> 	return { 
>         ...toRefs(obj) 				//相当于：先用toRefs把obj变成普通对象，在拓展开，然后依次对其属性如person进行reactive包装
>     }			
> }
> 
> //外界就可以正常使用person.name，同时person.name也是响应式数据，可以实现双向绑定
> ```
>

### computed()

> 作用：与Vue2的计算属性一样，因此向外界引用时也不需要带括号
>
> 引用：`import { computed } from "vue"`
>
> 语法：`computed( ()=>{} )`		或  	`let xxx = computed( ()=>{} )`
>
> 注：必须在setup的return对象中加入；computed的回调函数必须有return
>
> 如：
>
> ```js
> setup(){
>    	let test = computed(()=>{
>    	    return xxx;
>    	})
> 	return { 
>         	test			
>     	}			
> }
> ```
>
> 

### watchEffect()

与watch的区别：

> 1、立即执行：当页面一打开时就可以侦听到指定数据（而watch这样需要使 immediate:true ）
>
> 2、watchEffect不需要传递侦听的内容，因为会自动侦听watchEffect函数内的数据
>
> 3、缺点：无法侦听到旧值

> 引用：`import { watchEffect } from "vue"`
>
> 语法：`watchEffect( () => {} )`
>
> 停用watchEffect：直接调用该watchEffect
>
> ```js
> let unwatchEffect = watchEffect( () => {
>     xxxx;
> })
> 
> unwatchEffect			//此时就可以停用该侦听器了
> ```
>
> 

### watch()

是Vue2中的$watch 和 Vue3的watchEffect的结合

> 引用：`import { watch } from "vue"`
>
> 语法：`watch( data, callBack , {} )`
>
> 参数：
>
> > data：需要监听的值，如果不写，则默认自动监听callBack内的数据
> >
> > callBack： `(newVal, oldVal) => {}`	可以有两个形参			没有data的情况下，必须有变化的数据（即：cb里面不能只有形参newVal和oldVal），不然无法watch侦听
> >
> > {}：和Vue2一样，含 deep 和 immediate 属性		（当直接侦听一个响应式对象时，侦听器会自动启用深层模式：deep为true，immediate默认为false）
>
> 如：
>
> ```js
> import { ref, watch } from "vue";
> let num = ref(0);
> 
> watch(
>     num,
>     (newVal, oldVal) => {
>         console.log("num:", newVal);
>     },
>     {
>         deep: true
>     }
> );
> 
> setInterval(() => {
>     num.value++;
> }, 2000);
> ```
>
> 停用watch：与停用watchEffect一样
>
> 注：
>
> > 1、立即执行
> >
> > 2、当watch括号内不传参时，默认自动监听函数内的数据
> >
> > 3、监听对象的属性：对于data参数，变为 `() => obj.name`（意思为：`let s = toRefs(obj);` data参数变为s.name ），此时就可以深度监听obj.name的值了
> >
> > 4、多个监听：将data变为数组，与之对应的cb而需要改为：`([newVal1, newVal2], [oldVal1, oldVal2] => { console.log(newVal1) })`

### h()

类似于createElement（见 `../JS笔记/02-DOM.md` 中 `节点操作-> 增删改查 -> 创建doucument.creatElement` ）

> 作用：创建虚拟 DOM 节点
>
> 引用：`import { h } from "vue"`
>
> 语法：`h(type, props?, children?)`
>
> 参数：
>
> > type：可以是字符串（如div，表示创建div），也可以是vue组件
> >
> > props：对象类型，表示需要传递给虚拟子节点的各种属性（如id、class）、文本等
> >
> > children：该dom的子节点

## Vue3的生命周期

在Vue3中，已经没有Vue实例创建的生命周期了（即beforeCreate和created），因为setup就是一个创建的入口函数（即setup是在beforeCreate之前）

注：在Vue3中，可以使用Vue2的生命周期，但是其不能写在setup函数里面

### onBeforeMount()

> 定义：挂载之前，同Vue2的beforeMount
>
> 引用：`import { onBeforeMount } from "vue"`
>
> 语法：`onBeforeMount(() => {})`

### onMounted()

> 定义：挂载好了，同Vue2的Mounted
>
> 引用：`import { onMounted } from "vue"`
>
> 语法：`onMounted(() => {})`

### onBeforeUpdate()

> 定义：数据更新之前，同Vue2的beforeUpdate
>
> 引用：`import { onBeforeUpdate } from "vue"`
>
> 语法：`onBeforeUpdate(() => {})`

### onUpdated()

> 定义：数据更新好了，同Vue2的updated
>
> 引用：`import { onUpdated } from "vue"`
>
> 语法：`onUpdated(() => {})`

### onBeforeUnmount()

> 定义：Vue实例被销毁之前，常用于v-if，同Vue2的beforeDestroy
>
> 引用：`import { onBeforeUnmount } from "vue"`
>
> 语法：`onBeforeUnmount(() => {})`

### onUnmounted()

> 定义：Vue实例被销毁了，常用于v-if，同Vue2的destroyed
>
> 引用：`import { onUnmounted } from "vue"`
>
> 语法：`onUnmounted(() => {})`

## 父子通讯

### provide & inject

父组件通过provide将需要传的值传出去；子组件通过inject来获取并使用值

provide可以传给子，也可以传给孙等

#### provide

> 引入：`import { provide } from "vue"`
>
> 语法：`provide(key, value)`
>
> 参数：
>
> > key：传递数据的名字，自定义的，string类型
> >
> > value：需要传递的值

#### inject

> 引入：`import { inject } from "vue"`
>
> 语法：`inject(key)`
>
> 使用：直接 `let val = inject(key)`
> 

### props父传子

父传子中，需要传的数据不需要是响应式的，同时在子组件接收到数据的时候，会自动将其转化为响应式

#### props

> 作用：获取组件传值（与Vue2的props类似）
>
> 语法： 
>
> ```ts
> const props = defineProps({
>   	child: {
>             type:String, // 参数类型
>             default: 1, //默认值
>             required: true, //是否必传
>             validator: value => {
>                   return value >= 0 // 除了验证是否符合type的类型，此处再判断该值结果是否符合验证
>             }
>   	},
>   	sda: String, //undefined
>   	strData: String,
>   	arrFor: Array
> })
> 
> //子组件声明了的 props ，若父组件未传，则该值为 undefined 
> ```
>
> 

#### defineProps 

单独使用该api，只能设置是否必传和参数类型

> 语法：`defineProps()`			括号内的值就是从父组件接收到的值
>
> 如：
>
> ```ts
> const props = defineProps<{
>   	either: '必传且限定'|'其中一个'|'值', // 利用TS：限定父组件传 either 的值
>   	child?: string|number,
>   	strData?: string,
>   	arrFor: any[]
> }>();
> console.log(props);
> 
> //相较于props，该写法只能设置参数类型，但没有默认值
> ```
>
> 

#### withDefaults

> 作用：给defineProps设置默认值（因此必须搭配defineProps一起使用）
>
> 语法：`withDefaults(defineProps<Props>(), {})`		第二个参数就是默认值设置
>
> 如：
>
> ```ts
> interface Props {
> 	either: '必传且限定'|'其中一个'|'值', // 利用TS：限定父组件传 either 的值
>   	child: string|number,
>   	sda?: string, // 未设置默认值，为 undefined
>   	strData: string,
>   	msg?: string
>   	labels?: string[],
>   	obj?:{a:number}
>   }
> const props = withDefaults(defineProps<Props>(), {
> 	msg: 'hello',					//此时就能接受来自父组件传的msg等值了，同时msg、labels、obj也有默认值
>   	labels: () => ['one', 'two'],
>   	obj: () => { return {a:2} }
>   })
> 
> ```
> 
>注：**默认值为引用类型的，需要包装一个函数 return 出去**

### defineEmits

> 作用：子组件向父组传递件事件（与Vue2的$emit类似）
>
> 使用：
>
> > 子组件
> >
> > ```vue
> > <template>
> >   	<h1>{{ msg }}</h1>
> >   	<button @click="deliver">点击我调用父组件方法</button>
> > </template>
> >  
> > <script setup lang="ts">
> > 	import { ref } from "vue";
> > 	// props
> > 	const props = defineProps({		//父传子了msg
> >   		msg: {
> >    			type: String,
> >    			default: "",
> >   		},
> > 	});
> > 	// emit
> > 	const emit = defineEmits(["handleClick"]);
> > 	// methods
> > 	const deliver = () => {
> >   		emit("handleClick", "父组件方法被调用了");
> > 	};
> > </script>
> > ```
> >
> > 父组件
> >
> > ```vue
> > <template>
> >   	<HelloWorld :msg="msg" @handleClick="do"/>
> > </template>
> >  
> > <script setup lang="ts">
> > 	import { ref } from 'vue';
> > 	import HelloWorld from './components/HelloWorld.vue'
> > 	//data
> > 	const msg = ref('欢迎使用vite！')
> > 	//methods
> > 	const do = (params) => {
> >   		console.log(params);
> > 	}
> > </script>
> > ```

### defineExpose

> 作用：组件暴露自己的属性（原因：在Vue2中可以通过`this.$refs.xxx`或者`this.$parent`链获取到的组件的公开实例，但是使用`<script setup>` 的组件是默认关闭的）；使父组件能够使用到子组件的方法
>
> 如：
>
> ```js
> const handleClick2 = () => {
>   	a.value = 3
>   	obj.name = '我叫改变'
> };
> 
> defineExpose({
>      a,
>      obj,
>      handleClick2
>    })
> 
> //亦或
> const a = 1
> const b = ref(2)
> 
> defineExpose({
> 	a,
>   	b
>   })
> ```
> 
>

------

# 其他

## 缓存机制

利用v-if进行页面跳转时，会清空当前页面信息，如果需要保留数据，则需要利用缓存机制

> 在子组件的外边包上一个`<keep-alive></keep-alive>`的标签
>
> 路由则是在`<router-view>`外边包上`<keep-alive>`
>
> 最后，在routes里面添加
>
> ```js
> meta: {
> 	title:'测试tab页面切换的缓存问题',
> 	keepAlive: true 
> }
> ```
>
> 

## 懒加载

### 路由中

> 定义：路由懒加载，用到时才加载
>
> 作用：使当前页面的网络负担减少，提高页面加载速度
>
> 使用：在component引入组件时，直接用import引入，即可实现懒加载
>
> 语法：`component: import( /* webpackChunkName: "xx" */ './xx.vue')` 
>
> 参数：
>
> > /* webpackChunkName: "xx" */	： 定义该组件在网络中的名字，如xx.js
> > 该处为一个注释，但是如果没有，则名字为 0.js
> >
> > './xx.vue'	： 引入的组件位置

## render函数

概念：用于编程式地创建组件虚拟 DOM 树的函数

特点：如果一个组件中同时存在 `render` 和 `template`，则 `render` 将具有更高的优先级













