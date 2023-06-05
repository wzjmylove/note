# dir

作用：打印返回元素的对象，以更好的查看里面的属性和方法

语法：console.dir()



# JS的DOM

## 事件

浏览器中的 JS 是以事件驱动为核心的一门语言，事件即是可以被 JavaScript 侦测到的行为，是一种**触发-响应**机制。

网页中的每个元素都可以通过行为触发相应的事件，如：点击按钮-->产生事件--->执行操作。

事件的三要素是：事件源、事件、事件处理程序

> 事件源：触发事件的元素，如按钮，光标
>
> 事件/事件类型：就是以什么方式触发，如鼠标点击、鼠标移动
>
> 事件处理程序：事件触发执行的代码

### 事件的注册

给元素添加事件，称为 注册事件 或 事件绑定

注册事件有两种方式：传统方法（DOM 0级事件绑定） 和 方法监听注册方式（DOM 2级事件绑定）

**DOM 0级事件绑定**

> 语法：dom.on + type = function ( ) { }
>
> 该方法必须在使用前加上 " on "
>
> 一个对象同一个事件类型多次绑定function时，只会执行最新的函数（后面的会覆盖之前的）
> 即：一个对象同一个事件类型只能绑定一个事件处理函数

参数

> dom：绑定的事件对象
>
> type：事件类型
>
> function：事件触发执行的函数

#### 传统方法（DOM 0级事件绑定）

##### 放在html的标签内

```html
<body>
	<img onclick="doClick1()" src="./img/1.png" alt="">
    
    <script>
     function doClick1() {
         btn.src = './img/2.png';
     }
</script>
</body>
```

需要将事件onclick写在img标签内，并绑定JS内的函数（记得加括号）

##### 放在JS内

```html
<body>
	<img src="./img/1.png" alt="">
    
    <script>
     	var btn = document.querySelector("img");
     	btn.onclick = function () {
     	    btn.src = './img/2.png';
     	}
	</script>
</body>
```

需要将事件onclick写在JS内，并传一个函数

##### DOM 0级 事件的取消

直接给事件赋值一个null

如：`btn.onclick = null`

#### 方法监听注册方式（DOM 2级事件绑定）

> 语法：`dom.addEventListener( 'type' , function () { doSometings; } , 布尔值)`
>
> 一个参数多次绑定同一事件类型，不会覆盖
> 即：绑定多个函数时，函数会依次触发
>
> 参数：
>
> > type：事件类型
> >
> > function：事件触发执行的函数（可以时具名函数，也可以是匿名函数）
> >
> > 布尔值：false和true，一般给false（默认为false）
> > false：表示冒泡阶段
> > true：表示捕获阶段

具名函数：

```html
<body>
	<img src="./img/1.png" alt="">
    <script>
     var btn = document.querySelector("img");
     btn.addEventListener('click', fn , false);		//此处的fn不需要加括号
     function fn() {
         btn.src = './img/2.png';
     }
</script>
</body>
```

匿名函数：

```html
<body>
	<img src="./img/1.png" alt="">
    <script>
     var btn = document.querySelector("img");
     btn.addEventListener('click', function() {
         btn.src = './img/2.png';
	}, false)
</script>
</body>
```

##### DOM 2级 事件的取消

语法：removeEventListener( ' 需要取消的事件类型 ' , 函数名称 , 布尔值 )

注：必须是具名函数，不然不知道删除哪一个函数

例：

```html
<body>
	<img src="./img/1.png" alt="">
    <script>
     var btn = document.querySelector("img");
     btn.addEventListener('click', fn , false);		//此处的fn不需要加括号
     function fn() {
         btn.src = './img/2.png';
     }
     btn.removeEventListener('click',fn,false);
</script>
</body>
```

### 基本事件类型

#### 鼠标

##### 鼠标点击事件

| **方法**    | **描述**                             |
| ----------- | ------------------------------------ |
| click()     | 按下鼠标（通常是按下主按钮）时触发。 |
| dblclick()  | 在同一个元素上双击鼠标时触发。       |
| mousedown() | 按下鼠标键时触发。                   |
| mouseup()   | 释放按下的鼠标键时触发。             |

click事件可以看成是两个事件组成的：用户在同一个位置先触发mousedown，再触发 mouseup。因此，触发顺序是，mousedown首先触发，mouseup接着触发，click最后触发。
双击时，dblclick事件则会在mousedown、mouseup、click之后触发。

##### 鼠标移动事件

| **方法**     | **描述**                                                     |
| ------------ | ------------------------------------------------------------ |
| mousemove()  | 鼠标经过触发（会触发多次）                                   |
| mouseout()   | 鼠标移除（会触发多次）                                       |
| mouseover()  | 鼠标离开触发                                                 |
| mouseenter() | 鼠标进入一个节点时触发，进入子节点不会触发这个事件（只触发一次，无事件冒泡） |
| mouseleave() | 鼠标离开一个节点时触发，离开父节点不会触发这个事件（只触发一次，无事件冒泡） |

mouseover事件和mouseenter事件，都是鼠标进入一个节点时触发。两者的区别是，mouseenter事件只触发一次，而只要鼠标在节点内部移动，mouseover事件会在子节点上触发多次。

##### 鼠标焦点事件

| **方法** | **描述**         |
| -------- | ---------------- |
| blur()   | 失去鼠标焦点触发 |
| focus()  | 获取鼠标焦点触发 |

#### 键盘

| **方法**   | **描述**                                                     |
| ---------- | ------------------------------------------------------------ |
| keyup()    | 键盘按键被松开时触发   持续触发  （能识别功能键）            |
| keydown()  | 键盘按键被按下时触发   持续触发  （能识别功能键）            |
| keypress() | 键盘按键被按下时并抬起触发  （不能识别功能键，如F1、shift、CTRL、方向键） |

#### 表单

| **方法** | **描述**                                                     |
| -------- | ------------------------------------------------------------ |
| focus()  | 表单元素 获得焦点                                            |
| blur()   | 表单元素  失去焦点                                           |
| change() | 当内容发生改变的时候 并 **失去焦点**触发，一般用在  input select 标签上 |
| input()  | 当内容改变的时候触发 (不兼容 低版本的 ie，但比change更常用)  |
| submit() | 当点击了submit  按钮后，form标签会触发这个事件。             |
| reset()  | 当点击了reset  按钮后，form标签会触发这个事件。              |
| select() | 当表单里面的文字被选中后触发，不仅表单，别的标签也能触发这个事件。 |

注：

> 可以使用 .focus() .blur() .submit() .reset() 方法通过js来完成各种操作。
>
> reset()：只适用于对form标签的重置，不适用于单个input标签  		如：`form.reset()`

#### 滚轮

| **方法** | **描述**                                                 |
| -------- | -------------------------------------------------------- |
| wheel    | 当前元素的滑轮滑动事件函数（只要滚动滑轮就触发）         |
| scroll   | 事件在元素滚动条在滚动时触发（滚动条开始动的时候才触发） |

wheel方法中：event参数最最重要的事就event.wheelDelta属性，表示滚动的方向。

> 鼠标往上滚，wheelDelta = 125	（数值可能因游览器不同而不同，因此只关注正负）
>
> 鼠标往下滚，wheelDelta =  -125

------

### DOM事件流

定义：事件发生时，会在元素节点之间按照一定的顺序进行向外、向内传播，页面在接收事件的顺序程即为事件流。

DOM 2级 事件流分为3个阶段：捕获阶段、当前目标阶段、冒泡阶段

> 事件冒泡：事件开始时由最具体的元素接收，然后逐级向上传播到到 DOM 最顶层节点的过程。（如：点击了子元素，其父元素也会认为是被点击了）
> 注：有些事件没有冒泡，如 mouseleave，blur，focus，mouseenter
>
> 当前目标阶段：当前正在做的事情（如 点击 这一个动作）
>
> 事件捕获：由 DOM 最顶层节点开始，然后逐级向下传播到到最具体的元素接收的过程

------

### 事件对象

定义：事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象event，它有很多属性和方法。

如：

> 谁绑定了这个事件
>
> 鼠标触发事件的话，会得到鼠标的相关信息，如鼠标位置
>
> 键盘触发事件的话，会得到键盘的相关信息，如按了哪个键

在事件发生时（如鼠标点击后），所触发的函数会带有事件对象，并且该对象会赋值给函数的第一个形参

如：

```html
<body>
    <button class="btn">点我</button>
    <script>
        var btn = document.querySelector('.btn');
        btn.addEventListener('click', function(event, a, b) {
            console.log("event:", event);
            console.log('a:', a);
            console.log('b:', b);
        })
    </script>
</body>
```

结果：

![js_事件对象_result](.\image\js_事件对象_result.png)

#### 常见属性和方法

| **方法**            | **描述**                                      |
| ------------------- | --------------------------------------------- |
| e.target            | 返回 触发 事件的对象  标准                    |
| e.srcElement        | 返回 触发 事件的对象  非标准 ie6-8            |
| e.type              | 返回事件的类型 比如  click mouseover 不带  on |
| e.preventDefault()  | 阻止默认行为。                                |
| e.stopPropagation() | 阻止事件冒泡                                  |

##### this 和 target 

冒泡阶段：

> this：指向标签本身（含文本）
>
> e.target：指向标签本身（含文本）

捕获阶段：

> this：指向标签本身（含文本）
>
> e.target：指向所触发的地方（如，点击了子元素，则显示子元素标签）（比如点击子元素，父元素的监听回调函数中的e.target就指向子元素）

##### 阻止默认事件 ` e.preventDefault()`

阻止事件发生时的默认行为
如 点击`<a>`标签时，会跳转到其herf的地址，在阻止其发生后，点击`<a>`标签后不会发生跳转

```html
<body>
    <a href="www.baidu.com">百度一下</a>
    <script>
        // 1.阻止链接标签跳转
        var A = document.querySelector('a');
        A.addEventListener('click', function(event) {
            event.preventDefault();
        })
        
        // 2.阻止右击菜单选项
        document.addEventListener('contextmenu', function(event) {
            event.preventDefault();
        })
        
        // 3.阻止选中页面文字
        document.addEventListener('selectstart', function(event) {
            event.preventDefault();
        })
    </script>
</body>
```

注：e.returnValue = false 也能进行阻止默认事件，并且兼容ie

##### 阻止事件冒泡 `e.stopPropagation()`

正常情况下，若父元素和子元素触发条件相同，则在触发子元素时，父元素也会触发；添加了阻止事件冒泡后，可以阻止父元素触发

例：

```html
<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <script>
        var father = document.getElementsByClassName('father');
        var son = document.getElementsByClassName('son');
        
        father[0].addEventListener('click', function(event) {
            console.log(father[0]);
        });

        son[0].addEventListener('click', function(event) {
            event.stopPropagation();
            console.log(son[0]);
        })
    </script>
</body>
```

#### 鼠标事件对象

| 属性      | **描述**                                             |
| --------- | ---------------------------------------------------- |
| e.clientX | 鼠标经过触发。                                       |
| e.clientY | 鼠标进入一个节点时触发，进入子节点不会触发这个事件。 |
| e.pageX   | 鼠标离开触发。                                       |
| e.pageY   | 鼠标移除。                                           |
| e.screenX | 鼠标离开一个节点时触发，离开父节点不会触发这个事件   |
| e.screenY | 按下鼠标（通常是按下主按钮）时触发。                 |

> client：鼠标可视区
>
> page：鼠标在页面文档的位置
>
> screen：鼠标在电脑屏幕（整个显示器屏幕）的位置

该事件可以实现图标跟随鼠标移动等功能(上述都是显示鼠标的位置)

#### 键盘事件对象

| 属性      | **描述**                               |
| --------- | -------------------------------------- |
| e.keyCode | 得到按键的值，ASCII码                  |
| e.key     | 对应键盘的值     如：按键 r，key就为 r |

##### 组合键使用

关键功能键属性

> 以下属性值，**false为没按，true为按了**
>
> ctrlKey：CRTL键
>
> shiftKey：Shift键
>
> altKey：ALT键
>
> metaKey：Window键

如： shift + ~
`if(e.keyCode === 192 && e.shiftKey)`

### 事件委托

定义：指定一个事件来处理程序。	事件委托也称为事件代理， 在 jQuery 里面称为事件委派

作用：提高程序的性能，因为只操作了一次 DOM 

简单来说：不给予子元素注册事件，只给父元素注册事件，把处理代码放到父元素中执行（利用了e.target的特性）

例：

```html
<body>
    <ul>
        <li>陈一</li>
        <li>戴二</li>
        <li>张三</li>
        <li>李四</li>
        <li>王五</li>
    </ul>
    <script>
        var UL = document.querySelector('ul');
        //鼠标移入
        UL.addEventListener('mousemove', function(e) {
            e.target.style.backgroundColor = 'lightgreen';	//e.target此时是指向ul的子元素li的
        })
        //鼠标移出
        UL.addEventListener('mouseout', function(e) {
            e.target.style.backgroundColor = '';
        })
    </script>
</body>
```

结果：

![js_事件委托_result](.\image\js_事件委托_result.png)

------

## 基础DOM操作

### document.

用documen.getElementById ( " " )等来获取html中的标签

其中getElemen就是获取，By是通过什么方式，Id / ClassName / TagName等是标签的命名类型（如id、class）

注：打印出来是整个标签

### 操作类容

#### innerHTML

会解析标签

![dom_inner](.\image\dom_inner.png)

结果：														![dom_inner_result](.\image\dom_inner_result.png)

即，在原有标签的基础上又新增了一个标签

#### innerText

不会解析标签，且去除空格和换行

![dom_innerText](.\image\dom_innerText.png)

结果：															<img src=".\image\dom_innerText_result.png" alt="dom_innerText_result" style="zoom:150%;" />

即，只打印文字，不会新增一个标签

------

## 标签的获取

标签及其属性都是可读可写的

方法的括号内只需要写其类名或id名等即可，无需加 .  # 等

### 通过id获取

id是一个单数，在使用innerHTML与IinnerText的时候不需要加下标index

语法：document.getElementById ( ' ' );

### 通过class获取

语法：document.getElementsByClassName( ' ' );

class获取的方式是一个集合（**伪数组**），要想获取集合中的某一个标签，需要通过下标获取（方式同数组）

注：就算只有一个标签，也需要有下标 ‘ 0 ’ (index = 0)

tagname同class

如：`let div = document.getElementByTag('div')   div[0] = xxx`

### 通过name获取

用name的没法获取内容，如：

![dom_getName_bad](.\image\dom_getName_bad.png)

结果：

![dom_getName_bad_result](.\image\dom_getName_bad_result.png)

——————————————————————————————

用value可以获取到：

![dom_getName_good](.\image\dom_getName_good.png)

结果：

![dom_getName_good_result](.\image\dom_getName_good_result.png)

即，name的value获取的方式也是一个集合（**伪数组**），并且通过value可以获取和设置input等标签的内容

如：`iPt [0].value = '牛逼！';`		结果就会改变input里面的value

注：只有input这种才用value来获取和改变标签内容，其他全部用innerHTML与innerText

### 通过其他的选择器获取 ：后代、子代、兄弟

#### documen.getElementByTagName

> 语法：`documen.getElementByTagName('标签名')`
>
> 参数：标签名			如 p，span，div
>
> 作用：通过标签名获取元素
>
> 返回值：元素对象的集合（**伪数组**，其数组中的元素是对象）
>

例：获取 ul 下的 li

```html
<body>
    <ul>
        <li>火锅</li>
        <li>汉堡</li>
        <li>炸鸡</li>
        <li>烤肉</li>
    </ul>

    <ol>
        <li>可乐</li>
        <li>奶茶</li>
    </ol>

    <script>
        var ul = document.getElementsByTagName('ul');
        console.log(ul);
        //连续打点调用
        var ul_list = ul[0].getElementsByTagName('li');
        console.log(ul_list);
    </script>
</body>
```

结果：![dom_getTagName_result](.\image\dom_getTagName_result.png)

#### querySelector

（H5新增）

> 语法：`document.querySelector('')`		或者		`XX.querySelector('')`	（作用：获取XX下的第一个子元素）
>
> 返回与该模式匹配的**第一个元素**
>
> 注：
>
> > 1、**括号内如果是id，必须加 #	；	如果是class，必须加 .		否则会被当成标签处理**
> >
> > 2、括号内可以选择父级元素下的某个子集元素		如：`document.querySelector('table > tbody')`，获取table标签下的tbody标签

例：获取ul下第一个 li

```html
<body>
    <ul>
        <li>火锅</li>
        <li>汉堡</li>
        <li>炸鸡</li>
        <li>烤肉</li>
    </ul>

    <ol>
        <li>可乐</li>
        <li>奶茶</li>
    </ol>

    <script>
        var aa = document.querySelector('ul li')
        console.log(aa);
    </script>
</body>
```

#### querySelectorAll

（H5新增）

> 语法：`document.querySelectorAll('')`		或者		`XX.querySelectorALL('')`	（作用：获取XX下的所有子元素）
>
> 返回：与该模式匹配的**所有元素**，且为一个**伪数组**
>

### 特殊标签的获取

特殊标签有head、body、html等

返回html元素：document.documentElement

返回body元素：document.body

返回head元素：document.head

返回标题内容元素：document.title，并且何以更改标题 `document.title = 'xxx'`

------

## 标签的属性或样式更改

### 属性设置

#### 方法一：打点修改

> 语法：`dom.属性名 = xxx;`
>
> 如：
>
> ```js
> var btn = document.querySelector("btuuon");	//获取点击
> btn.onclick = function(){
>      btn.value = '属性value改变'
> }
> ```
>
> 注：
>
> > 给`<a>`标签触发事件时，需要取消默认事件	
> >
> > 方法：执行一个空事件：
> >
> > > 给`<a>`的`herf`一个`javascript:;`	
> > >
> > > 或者	给`<a>`的`herf`一个`javascript:void(0);`

#### 方法二：setAttribute

> 语法：`dom.setAttribute(prop, value)`
>
> 参数：
>
> > prop：属性名
> >
> > value：属性值

### 样式设置

#### 设置行内样式

可以在标签获取后（即用`document.getElementsByXXX`）后，在其后面用`.style`来选择样式

##### 单个样式设置

> 语法：`dom.style = ''`
>
> 注：凡是CSS样式里面有 - 的，都取消掉 - ，换成**驼峰命名** ；等号后面必须有引号

例：

```js
var bg = document.body;
bg.style.backgroundColor = '#FADCFA';
bg.style.fontSize = '20px';
```

##### 复合样式设置

批量修改：

> 语法：	以下xxx一般为 `属性值:属性名`，如 `color: red`
>
> > 1、`XXX.style.cssText = 'XXX'  +  'XXX' + ....`	(可以换行)
> >
> > 2、`XXX.style.cssText = 'XXX ; XXX'`	（不能换行）
> >
> > 3、es6中的模板字符串：
> >
> > ```js
> >  XXX.style.cssText = `XXX ; XXX ;`		//(可以换行)
> > ```
> >
> >  （ 反引号` ）（目的：防止引号冲突）
>
> 作用：做到对一个标签的多个样式进行修改（保留CSS中不和JS冲突的样式。即CSS里有长宽但JS没有则保留CSS的长宽）
>
> 注：也需要驼峰命名法

如：

```js
btn.style.cssText = 'color: red' + 'backgroundColor: pink'
```

### 设置元素的类名（className）

> 语法：`XXX.className = '类名1 类名2'`
>
> 注：
>
> > 1、不需要加 ' . ' 
> >
> > 2、且可以增加多个（直接空格隔开就好）
> >
> > 3、使用行内样式较多，类名较少

例：`btn.className = 'cleanfix bd'`				此时btn所绑定的标签会多cleanfix和bd两个类名

#### 覆盖类名的隐患和解决办法

给标签追加类名时，用该方法（`xxx.className = 'xxxx'`）会导致类名覆盖

解决办法：

> `xxx.classList.add('xx')`	追加名为xx的类（这是一个方法）
>
> `xxx.classList.remove('xx')`		删除xx类
>
> `xxx.classList.toggle('xx')`		切换xx类

------

## 节点操作

### 概述

节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。

> 元素节点 nodeType 为 1 		
>
> 属性节点 nodeType 为 2 
>
> 文本节点 nodeType 为 3 （文本节点包含文字、空格、换行等）

注：节点操作主要操作的是元素节点；节点值一般为空

例：

```html
<body>
    <div id="app">app div</div>
    <script>
        // 获取元素
        var app = document.getElementById('app');

        console.log(app.nodeType);		//1 --->元素节点
        console.log(app.nodeName);		//DIV 元素标签名
        console.log(app.nodeValue);		//null  , 始终为空

        console.dir(app);
    </script>
</body>
```

### 节点层级

节点存在嵌套关系，关系主要有：同级关系(兄弟)，上下级(父子)

#### 父节点获取`parentNode`

一个节点只有一个父节点。父节点最顶端是是 document

得到的是距离元素最近的父节点,如果如果找不到就就返回为null

注：parentElement也是寻找父节点，但是有的浏览器不兼容

例：

```html
<body>
    <div class="app">
        <div class="box">
            <span class="txt">span标签</span>
        </div>
	</div>

    <script>
        // 获取span标签的父节点
        var span = document.querySelector('.txt');
        console.log(span.parentNode);	//span节点的父节点是 box
        var app = document.querySelector('.app');
        console.log(app.parentNode);	// app的父节点 是  body
    </script>
</body>
```

#### 子节点获取

##### ChildNodes

获取所有的子节点，包含元素节点、文本节点

例：

```html
<body>
    <ul>
        <li>一</li>
        <li>二</li>
        <li>三</li>
    </ul>

    <script>
        var ul = document.querySelector('ul');//ul标签
        var list = ul.querySelectorAll('li');
        console.log('list:',list);
        
        // 获取所有子节点 --> 包含了元素节点 , 文本节点
        console.log(ul.childNodes);
        // 获取第一个子节点  [0]的话是文本节点
        console.log(ul.childNodes[1]);
    </script>
</body>
```

![dom_子节点获取_result](.\image\dom_子节点获取_result.png)

##### Children

获取所有的子节点，只拿元素节点		**常用**

例：

```html
<body>

    <ul>
        <li>一</li>
        <li>二</li>
        <li>三</li>
    </ul>

    <script>
        var ul = document.querySelector('ul'); //ul标签
        var list = ul.querySelectorAll('li');
        console.log('list:', list);

        // children ---> 开发中常用的
        console.log(ul.children);

        // 获取第一个字节 或 者最后一个子节点
        /* 
            firstChild : 第一个 子节点, 包含 文本节点和元素节点
            lastChild :  最后一个 子节点, 包含 文本节点和元素节点
            
            children[0] : 第一个
            children[XXX.children.length - 1] : 最后一个
        */

        // 包含文本 和 元素节点
        console.log('1:', ul.firstChild);
        console.log('last:', ul.lastChild);
        console.log('------------------------');
        // 获取元素的子节点, 只包含 元素子节点
        console.log('1:', ul.children[0]);
        console.log('last:', ul.children[ul.children.length - 1]);
    </script>
</body>
```

#### 同级节点

##### 包含 文本节点 和 属性节点

注：一般一个标签，`[0]`是标签内的文本，`[1]`才是标签本身（标签本身含文本）

> 上一个兄弟`previousSibling`
>
> 下一个兄弟`nextSibling`

##### 不包含 文本 的同级节点

> 上一个兄弟`previousElementSibling`
>
> 下一个兄弟`nextElementSibling`

### 增删改查

#### 增加节点

##### 直接增加并显示

###### document.write

> 语法：`document.write('')`
>
> 作用：增加 标签 或 文本
>
> 注：通过这种方式创建的节点，当页面加载完成之后再调用该方法，并会**导致页面重绘**（会覆盖原本的标签）	因此不常用

例：

```js
doucument.write('这是一个文本');
doucument.write('<p>这是P标签</p>');
```

###### innerHTML

> 语法：`dom.innerHTML = ''`
>
> 作用：增加标签，详情在最上面
>
> 注：直接调用`dom.innerHTML`是获取的是该标签内的 文本
> 因此在**直接赋值时会将原有标签的文本覆盖掉**（解决办法：**拼接 +=**）

##### 增加后不显示

常搭配下列位置添加一起使用

###### 创建文档树

> 语法：`document.createDocumentFragment();`
>
> 作用：可以创建一个新的空白文档片段，类似于主dom树；用于包裹一整块节点，方便后面添加这一整块节点

###### 创建`doucument.creatElement`

> 语法：`doucument.creatElement('')`
>
> 作用：创建节点
>
> 注：
>
> > 1、他只适用于创建元素；并且在未给其添加的位置时（即只创建），页面不显示			
> >
> > 2、若要添加文本内容，可以结合innerHTML使用
>

例：`var Li = document.createElement('li')`

###### 添加

> 语法：
>
> > 前面追加：`node.insertBefore(child , 指定元素)`
> >
> > 后面追加：`node.appendChild(child , 指定元素)`
>
> 参数：
>
> > node：父级元素
> >
> > child：子级元素
> >
> > 指定元素：需要添加的位置（可以不写，不写则默认追加在最后）
>

如：`Li.appendChild('li')`

```html
//前面追加的案例
<body>
    <div id="parentDiv">
        <span id="child">span标签</span>
    </div>

    <script>
        //创建一个新的、普通的<p>元素
        var newP = document.createElement("p");
        newP.innerHTML = '新的p标签'

        var sp = document.getElementById("child");
        //获得父节点的引用
        var parentDiv = sp.parentNode;

        //在DOM中在sp之前插入其新的<p>元素
        parentDiv.insertBefore(newP, sp);
    </script>
</body>
```

#### 删除`removeChild`

> 语法：`node.removeChild(child)`
>
> 作用：从DOM中删除一个子节点，并返回删除的节点
>
> 注：必须指定要删除的子节点
>

#### 克隆（复制）`cloneNode`

> 语法：`node.cloneNode('参数')`
>
> 返回：返回调用该方法的节点的一个副本，也称克隆节点/拷贝节点
>
> 参数：
>
> > 默认：不传参，即false
> >
> > true：深拷贝		 复制标签及其里面的内容
> >
> > false：浅拷贝		只复制标签，不复制标签内的内容（即不包含text）
>

------

## 元素尺寸

语法：`dom.xxx`，如 `dom.offsetTop = ''`

### 偏移尺寸`offset`

offset 即偏移量，可以用来动态获取元素的偏移位置、大小

建议获取元素位置，而非宽高

主要属性：

| 属性值       | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| offsetWidth  | 返回自身的宽度（width + padding + boder），与他人无关（结果不带单位） |
| offsetHight  | 返回自身的高度（height + padding + boder），与他人无关（结果不带单位） |
| offsetTop    | 返回距离最近带定位父盒上方的偏移量，且从父亲的padding开始算，若父级没有定位则以body为准 |
| offsetLeft   | 返回距离最近带定位父盒左侧的偏移量，且从父亲的padding开始算，若父级没有定位则以body为准 |
| offsetParent | 返回父级盒子中最近的带有定位的父盒子节点，若父级没有定位则以body为准 |

### 客户端尺寸 `client`

client的相关属性用来获取元素的可视区信息，如元素的边框大小、元素大小等

获取元素大小/宽高

主要属性：

| 属性值      | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| clientWidth | 返回自身包括padding、内容宽度（width + padding）（结果不带单位） |
| clientHight | 返回自身包括padding、内容高度（width + padding）（结果不带单位） |
| clientTop   | 返回元素上边框大小（border - top）                           |
| clientLeft  | 返回元素左边框大小（border - left）                          |

注：宽高属性：若有滚动条`overflow = scoll`则宽高会有偏差（不计算滚动条的宽度），因为滚动条占位置

### 滚轮尺寸`scroll`

scroll 用来动态获取元素大小、滚动距离

获取滚动距离

| 属性值      | **描述**                                                     |
| ----------- | ------------------------------------------------------------ |
| scrollWidth | 返回自身实际宽度，不含边框，不含滚动条宽度（结果不带单位）   |
| scrollHight | 返回自身实际高度，不含边框，不含滚动条高度  （结果不带单位） |
| scrollTop   | 返回被卷去的上侧距离，数值不带单位                           |
| scrollLeft  | 返回被卷去的左侧距离，数值不带单位                           |

------

