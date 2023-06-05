# BOM

BOM 是浏览器为开发者提供的 JavaScript 浏览器对象模型（Browser Object Modle），BOM 与网页无关，主要针对浏览器窗口、子窗口（frame）。BOM 比 DOM 更大，它包含 DOM。

## Window对象

调用window下的方法可以省略`window.XXX`，除window常见事件

### window 常见事件

| **方法**      | **描述**         |
| ------------- | ---------------- |
| window.onload | 窗口加载事件     |
| window.resize | 窗口大小改变事件 |

onload方法：网页正常是从上往下执行代码，但如若将js代码放置在代码顶部，则会导致DOM方法获取不到页面的标签。
					 在加入onload方法后，将DOM代码放于onload方法的函数内，则可以执行DOM
					 因为 onload 是等页面内容全部加载完毕， 再去执行处理函数.

语法：`window.onload = function () {}`

onresize方法：是调整窗口大小加载事件, 当触发时就调用的处理函数。
						 经常利用这个事件完成响应式布局。 window.innerWidth 当前屏幕的宽度

### 定时器

| **方法**        | **描述**                                                     |
| --------------- | ------------------------------------------------------------ |
| setTimeout()    | 设置一个定时器，该定时器在定时器到期后执行一个函数或指定的一段代码（一次性） |
| setInterval()   | 重复调用一个函数或执行一个代码段，在每次调用之间具有固定的时间延迟（循环执行） |
| clearInterval() | 清除先前通过调用  setInterval()  建立的定时器                |
| clearTimeout()  | 清除先前通过调用 setTimeout() 建立的定时器                   |

 注：

> 1、定时器可以用表达式的方法进行取名，这样可以区分不同的定时器，也方便实现清除定时器
> 每个定时器都会有自己的编号，编号从数字1开始；因此名字即为编号，编号即为名字
>
> 2、定时器的取名相同时，并不会覆盖，而是都会执行（因为他们编号不同）
>
> 3、获取编号的方法：直接打印定时器，结果即为该定时器编号	如：`console(setTimeout(fn,1000))`
>
> 4、清除定时器的方法是通用的，即clearTimeout可以清除setInterval建立的定时器
>
> 5、清除定时器可以输入定时器名字或编号
>
> 6、清除同名定时器时，需要输入其编号（如果输入的是名字，则清除最后一个定时器）
>
> 7、一个定时器只能给一个对象使用，多个对象调用同一个定时器时只有最后一个对象能够使用
> 解决办法：给该对象追加一个属性，属性值就为需要使用的定时器

#### setTimeout ( )

只执行一次，在 time 毫秒后执行一次

> 语法：` setTimeout( function(){} , time , 传参)`
>
> 参数
>
> > function：回调函数（一个函数处理另外一个函数时，这个function就叫处理这个事情的回调函数），可以是匿名，也可以是具名
> > 	 		 （如：` setTimeout(fn,time,传参)  ;  function fn(){}`）
> >
> > time：时间，所需的定时时间	单位：ms		（a timeout less than 4ms is increased to 4ms when the nesting level is greater than 5，即嵌套超过5层后计时器至少会延迟4ms）
> >
> > 传参：所传递的参数，可以是多个（多个时需要用 逗号 隔开）
>

例：

```js
setTimeout(Fn, 1000, 'amd', 'yes');
function Fn(a, b) {
	alert(a + b);
}
```

#### setInterval ( ) 

循环定时器，每隔 time 毫秒执行一次

语法：` setInterval( function(){} , time , 传参)`

参数

> function：回调函数（一个函数处理另外一个函数时，这个function就叫处理这个事情的回调函数），可以是匿名，也可以是具名
> 	 		 （如：` setTimeout(fn,time,传参)  ;  function fn(){}`）
>
> time：时间，所需的定时时间	单位：ms		不填写，则默认为10ms（且最低为10，低于10则默认为10）
>
> 传参：所传递的参数，可以是多个（多个时需要用 逗号 隔开）

### location 对象

window 对象给我们提供了一个 location 属性用于获取或设置窗体的 URL，并且可以用于解析 URL 。 因为这个属性返回的是一个对象，所以我们将这个属性也称为 location 对象。

属性：

| **属性**            | **描述**                                                     |
| ------------------- | ------------------------------------------------------------ |
| **location.href**   | 获取或者设置整个  url (使浏览器跳转页面)  （可读可写）（功能和a标签一样） |
| location.host       | 返回服务器名及端口号 如:  `127.0.0.1:8848`                   |
| location.port       | 返回端口号 如果没写返回空的字符串                            |
| location.pathname   | 返回路径                                                     |
| **location.search** | 返回?参数 如:  ?name=xiao                                    |
| location.hash       | 返回  # 后面的内容                                           |

方法：

| 方法               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| location.assign()  | 重定向，跟href一样,可以跳转页面(重定向页面) (可以实现后退)   |
| location.replace() | 不记录历史，直接替换当前页面  （不能后退）                   |
| location.reload()  | 不给参数：重新加载页面（从缓存中加载）；如果参数为true  强制刷新  ctrl + f5  （从服务器中加载） |

#### location.search

一般用于form表单提交的数据

例：

```html
<body>
    <form action="this.html">
        <input type="text" placeholder="请输入英文姓名" name="uname">		//输入MAX
        <input type="number" placeholder="请输入年龄" name="uage">		//输入18
        <input type="submit" value="提交">
    </form>
    <script>
        var URL = location.search;
        console.log(URL);
        //结果：?uname=MAX&uage=18
        //如果需要获取名字MAX，需要对其切片（如var arr = URL.substring(1).split('=')
    </script>
</body>
```

##### URLSearchParams对象

> 语法：`new  URLSearchParams ( '参数' )`
>
> 返回：返回一个对象；因此若要使用，可以**直接将其转换为字符串**
>

方法：

| 方法  | 描述         |
| ----- | ------------ |
| set() | 设置参数     |
| get() | 获取参数的值 |

方法的用法

> set：`set(key, value)`
>
> get：`var search = new URLSearchParams(location.search) ;	search.get('XX')`

如：                                                                                               

```html
<body>
    <form action="this.html">
        <input type="text" placeholder="请输入英文姓名" name="uname" value='MAX'>
        <input type="number" placeholder="请输入年龄" name="uage" value='18'>
        <input type="submit" value="提交">
    </form>
    <script>
        var URL = location.search;
        var search = new URLSearchParams(URL);
        document.write(search.toString());
        
        //get()获取参数的值
        var name = search.get('uname');
        //set()设置参数		此时URL为?uname=MAX&uage=18&hoby=sleep
        search.set('hoby','sleep');
    </script>
</body>
```

### navigator 对象

navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客户机发送服务器的 user-agent 头部的值(navigator 用于标识浏览器)

用户代理字符串（移动端）

phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone

| 属性                  | **描述**                  |
| --------------------- | ------------------------- |
| navigator.appCodeName | 浏览器名称，通常为Mozilla |
| navigator.userAgent   | 用户代理字符串            |

#### navigator.userAgent 

通常用来判断用户是用什么终端打开的页面（如移动端，pc端）

### history 对象

history 记录了用户曾经浏览过的页面(URL),并可以实现浏览器前进与后退相似导航的功能

| **方法**  | **描述**                                                     |
| --------- | ------------------------------------------------------------ |
| back()    | 后退功能                                                     |
| forward() | 前进功能                                                     |
| go()      | 前进后退功能,  参数是 1 前进一个页面，2则是前进两个页面；如果是 -1 后退一个页面 |

可以直接给按钮增加功能：`history.back();`	即可实现按下按钮后退

### window.getComputedStyle ( ) 方法

> 语法：`window.getComputedStyle(element, [pseudoElt]);`		window可以省略
>
> 参数：
>
> > element：必须，需要获取样式的对象
> >
> > pseudoElt，可选，指定一个要匹配的伪元素的字符串。必须**对普通元素省略**（或`null`、false）。
>
> 作用：返回一个对象，该对象在应用活动样式表并解析这些值可能包含的任何基本计算后报告元素的所有CSS属性的值。
> 即返回一个包含该对象所有CSS样式的一个合集

例：

```html
<style>
    div {
        width: 100px;
        height: 100px;
        background-color: deeppink;
    }
</style>

<body>
    <div></div>
    <script>
        var box = document.querySelector('div');
        var style = getComputedStyle(box, null).height;
        console.log(style);
        //结果：100px
    </script>
</body>
```

### File System Access API

https://www.qinwg.cn/n/fe/file-system-access-api.html

该API有的浏览器不支持，如safari、Firefox

可以通过检测window对象下是否具有以下方法来判断该浏览器是否支持API

```js
const supported = window?.showOpenFilePicker && window?.showDirectoryPicker;
console.log(supported ? "支持" : "不支持");
```

#### `window.showOpenFilePicker()`

> 作用：打开文件
>
> 语法：`window.showOpenFilePicker(multiple, excludeAcceptAllOption, types)`
>
> 参数：
>
> > multiple：布尔值；是否多选. 默认 `false`
> >
> > excludeAcceptAllOption：布尔值；是否排除`接受所有`选项. 默认 `false`
> >
> > types：数组；指定文件类型
>
> 返回：一个promise

如：

```js
// 选择文件
const openFile = (multiple = false) => {
  window
    .showOpenFilePicker({
      multiple: multiple, // 多选
      excludeAcceptAllOption: true,
      types: [
        {
          description: "选择图片",
          accept: {
            "image/*": [".png", ".gif", ".jpeg", ".jpg"],
          },
        },
      ],
    })
    .then((fileHandles) => {
      console.log(fileHandles);
      let files = fileHandles
        .map((fileHandle) => fileHandle.name || "")
        .join("\n");
      console.log(`选择了 ${fileHandles.length} 个文件: \n${files}`);
    })
    .catch((err) => {
      console.log("选择文件失败:", err);
    });
};
```



#### `window.showOpenSavePicker()`

保存文件

#### `window.showDirectoryPicker()`

打开目录

------

## JS实现动画

在css3中我们已经可以使用一些属性快速的做出来，但是有时候为了浏览器的兼容性我们还是需要使用js来制作网页中的动画（如轮播图）

### 匀速动画

1. 设置元素为绝对定位，只有绝对定位后，left,top等值才生效。

2. 动态改变值 , 如: 每次元素距离左侧 + 1

3. 这里使用setInterval() 不断重复这个操作。

4. 加一个结束定时器的条件  clearInterval() 方法可取消由 setInterval() 设置的交互时间。

即：匀速动画 就是 盒子是当前的位置 + 固定的值 

例：

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }
    
    #box {
        margin-top: 20px;
        width: 100px;
        height: 100px;
        background-color: rebeccapurple;
        position: absolute;
    }
    
    #wrap {
        margin-top: 150px;
        width: 100px;
        height: 100px;
        background-color: red;
        position: absolute;
    }
</style>

<body>
    <button id="btnR">向右移动</button>
    <button id="btnL">向左移动</button>
    <div id="box"></div>
    <div id="wrap"></div>
    
    <script>
        /*      1. 设置元素为绝对定位，只有绝对定位后，left,top等值才生效。
                2. 动态改变值 , 如: 每次元素距离左侧 + 1
                3. 这里使用setInterval() 不断重复这个操作。
                4. 加一个结束定时器的条件  clearInterval() 方法可取消由 setInterval() 设置的交互时间。

                注意: 匀速动画 就是 盒子是当前的位置 +  固定的值  
                
                多个 物体(元素) 一起运动
                    单定时器,存在问题. 不能每个元素 一个定时器
                    解决:
                        让定时器作为对象属性
                        直接使用 element.timer,将 下面的 timer 改成  element.timer
            */
        var btnR = document.getElementById('btnR');
        var btnL = document.getElementById('btnL');
        var box = document.getElementById('box');
        var wrap = document.getElementById('wrap');
        /**
         *  封装 匀速动画
         *  参数:
         *      element : 要执行动画的对象(元素)
         *      target  : 动画终止的条件
         *      callback : 回调函数,这个函数会在动画执行完成的时候执行
         */
        var timer = null;
        function startMove(element, target, callback) {
            // 解决 :重复点击速度会变快,在开始运动时,关闭已有的定时器
            clearInterval(timer)
            console.log(element, timer, element.timer);
            element.timer = setInterval(function() {
                var speed = 10; //把运动的数值 用变量保存
                // 判断距离目标的位置,达到自动变化数值正负
                if (element.offsetLeft < target) {
                    speed = speed;
                } else {
                    speed = -speed; //向左   left的值减小
                }
                // 停止条件, 直接在内部判断,判断到达目标值
                // 将运动 和停止 隔开
                if (element.offsetLeft === target) {
                    clearInterval(element.timer)
                        // 回调函数写在定时器结束里面，判断有没有这个参数,有就调用
                        // 使用 && 做判断
                    callback && callback();	//相当于 if(callback) { callback(); }
                } else {
                    element.style.left = element.offsetLeft + speed + 'px'
                }

            }, 15)
            console.log(element, timer, element.timer);
        }

        btnR.onclick = function() {
            startMove(box, 600, function() {
                // 向右走 改变元素的颜色
                box.style.background = "green"
                box.style.transition = '.3'
            })

            startMove(wrap, 300)
        }
        btnL.onclick = function() {
            startMove(box, 0)
        }
    </script>
</body>
```

### 缓冲动画

1. 设置元素为绝对定位，只有绝对定位后，left,top等值才生效。

2. 让盒子每次移动的距离慢慢变小， 速度就会慢慢落下来。 

3. 速度由距离决定, 速度=(目标值-当前值)/缩放系数    （缩放系数可以自己设定，如10）

4. 停止的条件是： 让当前盒子位置等于目标位置就停止定时器

即：缓冲动画就是 盒子当前的位置 + 变化的值(目标值 - 现在的位置) / 缩放系数）



