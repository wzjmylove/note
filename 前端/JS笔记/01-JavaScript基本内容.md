# JavaScript

## 关于前端的理论知识

### 游览器组成

#### 外壳（shell）

含界面、菜单、网络、控制台、调试功能等

#### 内核

> 渲染引擎	 作用：渲染网页
>
> JS引擎		作用：解析JS代码	（chrome游览器的JS引擎是V8）
>
> 其他模块（如：异步）

### 语言类型

#### 解释性语言

JavaScript	/	Python	/	PHP

解释一行，执行一行

优点：不需要编译成文件，能跨平台（即跨系统，windows或Linux系统都能用）

不足：只比编译性语言慢一点点

注：JS是一门既面向对象，又面向过程的语言
		与传统面向对象的语言相比，js缺少：类 接口 等

#### 编译性语言

C	/	C++

通篇翻译

优点：快

缺点：不能跨平台

### 线程

#### 单线程

一个执行体，同一时间只能做一件事情；如果需要做另外的一件事，必须停下手中的事才可以（同一时间只能做一件事）

注：JavaScript不是单线程的，但是其引擎是单线程的(一个浏览器进程中只有一个JS的执行线程，同一时刻内只会有一段代码在执行)
即：如果是手动打开浏览器或新建窗口，则会创建一个新的进程，里面也是有新的JS执行线程；但如果是在当前页面通过点击a标签等方法打开的新窗口（必须保证新窗口域名和之前相同），则不会创建新的进程

#### 多线程

一个执行体同时能做多件事情

### 同步与异步

JS中所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）

#### 同步

同步任务: 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务	

同步代码：for、赋值、console.log、if .......

#### 异步

> 单线程是异步产生的原因
>
> 事件循环是异步的实现方式

异步任务: 不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

异步代码：事件绑定 DOM、回调函数、图片加载、定时器、数据交互:ajax...、node 中的异步 API

JS的异步操作原理：异步机制是浏览器的两个或以上常驻线程共同完成的，例如异步请求是由两个常驻线程：JS执行线程和事件触发线程共同完成的，JS的执行线程发起异步请求（这时浏览器会开一条新的HTTP请求线程来执行请求，这时JS的任务已完成，继续执行线程队列中剩下的其他任务），然后在未来的某一时刻事件触发线程监视到之前的发起的HTTP请求已完成，它就会把完成事件插入到JS执行队列的尾部等待JS处理。又例如定时触发（settimeout和setinterval）是由浏览器的定时器线程执行的定时计数，然后在定时时间把定时处理函数的执行请求插入到JS执行队列的尾端（所以用这两个函数的时候，实际的执行时间是大于或等于指定时间的，不保证能准确定时的）

#### 执行过程

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为事件循环（event loop）：

<img src=".\image\js_同异步.png" alt="js_同异步" style="zoom:67%;" />

图片解释：异步执行时，先注册其回调函数，再放入Event Queue中，当主程序空的时候，执行Event Queue
				  即只有当主程序走完时，才会执行异步

例：

```js
console.log(1);
setTimeout(function() {
    console.log(3);
}, 110);
setTimeout(function() {
    console.log(2);
}, 100);
console.log(4);
//结果是打印 1 4 2 3
```

### JavaScript的组成

> ECMAScript：描述JavaScript的语法和基本对象
>
> DOM：文档对象模型，用于处理网页内容和方法
>
> BOM：浏览器对象模型
>
> WebAPI：DOM、BOM、AJAX
> DOM API、BOM API、AJAX API 都是浏览器内置的API

## 变量

命名规则

>  变量名必须以英文字母、_、$开头
>
>  变量名可以包含英文字母、_、$、数字
>
>  不可以用系统的关键字、保留字作为变量名

​																								**保留关键字**

![js_bianliang_keywords](.\image\js_bianliang_keywords.png)

注：

> 同时使用多个变量时，可以只用一个var，但多个变量名需要用 " , " 隔开
>
> 变量不声明，直接赋值，会隐式声明，会给全局变量
>
> 命名用驼峰命名

## 数据类型

数据类型:用来区分数据	这些称之为原始类型，不能在细分

es5	有6种：

​					基础数据：

​									number				 数值型			含NaN （NaN不等于任何数，包括他自己）

​									string					 字符串型

​									BooLean			   布尔值型		 true、false

​									undefined			  未定义													（undefined是变量，不过是全局的，因此可以 var 定义， 也可以在块作用域内 let 定义；因此为了获取到undefined，常用`void 0`）

​									null						空													

​					复杂数据：

​									object	

es6之后补充了两种基础数据类型：

​									symbol	符号(独一无二,防 止串名)

​									bigint	大整数型

### 基本类型

栈数据

number、string、BooLean、undefined、null、symbol

注：当两个基本类型复制时，只会复制其文本，因此任意一方改变**不会**影响到对方

### 引用类型

堆数据

object、Array、Function

当两个基本类型复制时，复制的是地址，因此任意一方改变**会**影响到对方；但如果对方重新赋值，而非在原有基础上修改，则同栈数据

例：

```js
var arr1 = new Array(5, 4, 3, 2);
var arr2 = arr1;
console.log(arr2);		//结果是[ 5, 4, 3, 2 ]
arr1.pop();
console.log(arr2);		//结果是[ 5, 4, 3 ]
```

重新赋值的例子：

```js
var arr1 = new Array(5, 4, 3, 2);
var arr2 = arr1;
console.log(arr2);		//结果是[ 5, 4, 3, 2 ]
arr1 = [1, 2, 3];
console.log(arr2);		//结果是[ 5, 4, 3, 2 ]
console.log(arr1);		//结果是[ 1, 2, 3 ]
```

### 检测数据类型

typeof

使用方法：typeof	变量名		或		typeof（变量名）

## 运算符

注意：尽量不要用浮点数进行直接运算，会有误差；同理浮点数不能直接比较

### 基本运算符

1、++a 前置自增运算符 : 先自加1，在返回值  如 ++age 等于 age = age + 1 

2、a++ 后置自增运算符 : 先返回原值，后自加1 如 `age = 10 , a = age++ + 10 , a = 20 , age = 11`

​    前后自增如果单独使用，效果是一样的

3、== : 等于符号，只要求值一致，会转型 如 18 == '18' , 返回true；NaN == NaN，返回false

4、=== : 全等于，要求两边数据类型和值完全一致 如 18 === '18' , 返回false

5、!=：==的反面

6、!==：===的反面

### 逻辑运算符

#### 三元表达式

条件表达式 ? 表达式1 : 表达式2    如果条件表达式结果为真则返回表达式1，反之则返回表达式2

```javascript
3 > 2 ?
    (
    	console.log('123'),
    	console.log('345'),
	)
    (
    	console.log('567'),
    	console.log('789'),
    )
//结果：会打印出123  345
```

#### 短路运算（逻辑中断）

原理 : 当有多个表达式（值）时，左边的表达式值可以确定结果时，就不再继续运算右边的的表达式的值

##### 逻辑与 : &&

语法 :   表达式1 && 表达式2

​       	 如果表达式1 true ，返回表达式2 ；如果表达式1 false ，返回表达式1

​        	如 : 123 && 456 , 返回456 ; 0 && 456 , 返回0

​        	(假命题：0 '' null  undefine  NaN)

##### 逻辑或 : ||

语法 :   表达式1 || 表达式2

​        	如果表达式1 true ，返回表达式1 (**表达式1执行，后面的表达式不执行**)；如果表达式1 false ，返回表达式2

​        	如 : 123 || 456 || 789 , 返回123 ; 0 || 456 || 822 , 返回456

#### 赋值运算符

+= : 如 num + = 2 	等于 num = num + 2 

 -=	 *= 	/=同理

#### 运算优先级

> A : ()
>
> B : 一元运算符 只涉及一个的算数运算 常用符号：++  -- !
>
> 注：二元运算符是非B类，二元运算符例：1 + 1 ； 三元运算符也是非B类，例子：条件表达式 ? 表达式1 : 表达式2    如果条件表达式结果为真则返回表达式1，反之则返回表达式2
>
> C : 算术运算符 先 * / 后 + -
>
> D : 关系运算符 >  >= <  <=
>
> E : 相等运算符 ==  != === !==
>
> F : 逻辑运算符 先 && 后 ||
>
> G : 赋值运算符 =
>
> H : 逗号运算符 ，

## 类型转换

### 强制转换/显式转换

#### 化为数值型

##### Number

如：

```javascript
number('123')	———>	123

number(true)	———>	1

number(flase / null / [] / '')	———>	0			 (空数组转换后为0)

number(undefined / '123abc' / {})	———>	NaN		 (空对象转换后为NaN)
```

注：就算是NaN，但是他们所有用typeof来看，都是number类型

##### parseInt

化整型

parseInt () 

如 `age = '123w' 	parseInt (age) = 123`

**指定，进制数**

将指定进制数转化为10进制

如 `parseInt('16',8) = 14					parseInt('1a',16) = 26	`
即：将8进制的数字16转化为10进制					将16进制的1a转化为10进制

##### parseFloat

化浮点型

parseFloat () 

如 `age = '1.23wz3.45' parseFloat (age) = 1.23` 

注：parseInt与parseFloat都是从数字开始看起，看到非数字为止，不同于Number（）。

#### 化为字符串型

##### String

不管所给的值是什么（如null、undefined），都会转化为字符串

用法：`var num = true ;	String(num) = 'true'`

##### toString

用法：`var num = true ;	num.toString() = 'true'`

注：

> **null和undefined不能调用toString的方法**
>
> toString()是可以带参数的，表示以多少进制格式返回数值字符串	如：toString(2)，表示传回2进制的字符串

#### 化布尔型

##### Boolean

用法：`Boolean(2) = true`

注：只有6大假值（NaN null undefined 0 ' ' flase）才会被转换为false

### 隐式转换

##### isNaN

检查一个值能否转化成数值类型，即判断是否为非数

当一个值不能被 Number 或 parseInt 或 parseFloat 转换成功时，就会返回true，表示该值无法被转为数字型

用法：`isNaN(NaN) = true`

##### 运算符

`+` 运算/拼接

```javascript
var a = 1 + '3'		//结果：13（字符串型）		拼接13，调用了String()
var b = 1 + +'3'	//结果：4（数字型）		 	 可以理解为“+3”，调用了Number()的功能，将数据转化为数字，转不了就得到NaN
var c = 1 + [1,2,3]	//结果：11,2,3（字符串型）	 如果有个对象，调用就是toString(),相当于将数组转化为字符串（），再进行拼接
```

其他的如 ' - '、' * '、' / '、' % ' 都是计算，调用Number()，不能实现拼接功能

##### 比较运算

string和number作比较时，string会被隐式转化为number类型

如null会被转换为0

注：

> NaN不能做比较，因为其是未知数
>
> 引用类型数据进行比较时，比较的不是值是否相等，而是比较他们指向的地址是否一样

------

## 作用域

定义：定义变量后，变量会在一定的范围内起作用，这个作用范围就是作用域

全局作用域：游览器中的全局作用域，就是window

局部作用域(函数作用域) ：定义在函数里面的变量，是局部变量，其作用范围只在函数内有效

### 作用域链

定义：内部函数访问外部变量，采取链式查找的方式来决定取哪个值，这种结构称之为作用域链

如：

```js
var a = 1;
function fn1() {
    function fn2() {
        console.log(a);
    }

    function fn3() {
        var a = 4;
        fn2();		//只是调用了fn2，fn3并没包含fn2，因此fn2的上一级作用域链是fn1
    }
    var a = 5;
    return fn3;
}
var fn = fn1();
fn();	//结果：5
```

![js-zuoyongyu](.\image\js-zuoyongyu.png)

------

## 预解析

js引擎运行js分为两步: 预解析	代码执行

> 1.预解析：预解析js引擎会把js里面所有的 var 还有 function 提升到当前作用域的最前面
>
> 2.代码执行：按照代码书写的顺序从上往下执行

预解析分为：变量预解析（变量提升）	函数预解析（函数提升）

> 变量提升：把所有变量提升到当前作用域最前面	不提升赋值操作
>
> 函数提升：把所有函数提升到作用域的最前面		不调用函数

例一：

```javascript
/*	 预解析前	*/
f1 () ;

console.log(c) ;
console.log(b) ;
console.log(a) ;

function f1 ( ) {
	var a = b = c = 9 ;
	console.log(a) ;
	console.log(b) ;
	console.log(c) ;
}
```

```js
/*	 预解析后	*/
function f1 ( ) {
		var a ;
		a = b = c = 9 ; //相当于var a = 9; b = 9 ; c = 9 ; b和c直接赋值没有var声明当全局变量看
		console.log(a) ;
		console.log(b) ;
		console.log(c) ;
	}
	f1 ( );
	console.log(c) ;
	console.log(b) ;
	console.log(a) ;

结果： 9  9  9  9  9  9  错误
```

例二：

```js
/*	 预解析前	*/
var a = 18 ;
f1 ( ) ;

function f1 ( ) {
	var b = 9 ;
	console.log (a) ;
	console.log (b) ;
	var a = '123' ;
}
```

```js
/*	 预解析后	*/
var a ;
function f1 ( ) {
	var b ;
	var a ;
	b = 9 ;
	console.log (a) ;
	console.log (b) ;
	a = '123' ;	
}
a = 18 ;
f1 ( ) ;

结果：undefined  9
```

### 全局预编译

1、创建GO对象

2、找变量声明,将变量声明作为Go对象的属性值,传进去,赋值为undefined

3、找函数式声明（**不会找函数表达式**，因为函数表达式相当于赋值）,赋值为对应的GO属性值

例：

```js
console.log(a);
var a;                  //声明a
a = 10;                 //a变量赋值
function a() {  }       //函数声明
var a = function(){};   //函数表达式
console.log(a);

结果：f a() {}
	 f () {}
```

```typescript
解析:
1.创建GO对象 ---> GO{}
2.找变量声明 ,赋值为undefined
	GO{
		a:undefined
	}
3. 找函数声明
	GO{
		a:undefined --> function a() {  };	//由变量变为函数（覆盖）
	}
4.执行的过程
	GO{
		a:undefined; ---> function a() {  }; ---> 10 --> function(){}	//赋值过程，先给a赋值为10，再覆盖为函数表达式
	}
```

### 函数预编译

1、创建AO 对象

2、找函数的形参和变量声明,将变量和形参作为AO属性名，值为undefined

3、把实参值和形参相统一

4、在函数体里面找函数声明（只找函数声明,不找函数表达式），赋值函数体

例：

```js
function b(x){
	console.log(x);
	var x = 123;
	console.log(x);
	function x (){};
	console.log(x);
	var c = function(){}
	console.log(c); 
}
b(22);

结果：ƒ x (){}
	 123
	 123
	 function(){}
```

```typescript
解析
1.创建AO对象 ---> AO{}
2.找函数的形参和变量声明,并赋值为undefined
	AO{
		x:undefined,
		c:undefined
3.把实参值和形参相统一
	AO{
		x:undefined; ---> 22
		c:undefined
	}
4.在函数体里面找函数声明(只找函数声明,不找函数表达式),并赋值为函数体
	AO{
		x:undefined; ---> 22 ---> function x (){}
		c:undefined
	}
5.开始执行代码
     AO{
          x:undefined; ---> 22 ---> function x (){} ---> 123
          c:undefined; ---> function(){}
     }
```

------

## 闭包

结合 	 ES6笔记 —> 块级作用域 —> 闭包

定义：当两个函数彼此嵌套，且内部函数引用了外部函数的变量（被引用的变量在局部变量被销毁时保留），那么内部的函数就是闭包

闭包的典型案例：

```js
function foo() { //外部函数
	// 局部变量 (foo 作用域)
	var name = 'wz' // name 被 bar 使用所以没有被垃圾回收机制回收
	var age = 20
	function bar() { //内部函数
		// 构成了一个闭包
		console.log(name);
	}
    return bar; // 返回 bar的引用
}
foo()();
//结果是打印了 wz			nmae因为被内部函数引用，所以没有被销毁，而age被销毁了
```

### 垃圾回收机制（GC）

JS中拥有自动的垃圾回收机制，会自动将垃圾对象从内存中销毁

我们不需要也**不能**改变垃圾回收操作

### 作用

1、访问函数内部的变量

2、变量私有化（即只允许变量在函数内部使用）

3、给一组dom循环绑定事件，保存每次循环的值	例：

```js
function test() {
    var arr = [];
    //循环给数组添加函数体
    for (var i = 0; i < 5; i++) {
        arr[i] = function() {
            console.log('NO.' + i);
        }							
    }
    return arr; //返回数组（数组是5个函数体）
}
//console.log(test())	返回一个数组。数组包含五个值，每个值都是一个函数体
var MyArr = test();
for (var i = 0; i < 5; i++) {
    MyArr[i]();		//相当于test()[i]()	而	test()[i]是一个函数体，后面必须加括号来执行
}

结果：NO.5
	 NO.5
	 NO.5
	 NO.5
	 NO.5
原因：因为闭包保存了i，而i的最终值是5，因此才会是5个“NO.5”
	 也可以理解为，在进行函数test内的循环时，只给数组arr添加了值（该值是函数体），并没有运行函数体（因为没有运行函数体的代码），因此i会加到5，进而闭包保存了i = 5
```

利用闭包保存，使其输出 0 1 2 3 4

```js
function test() {
    var arr = [];
    for (var i = 0; i < 5; i++) {
        //5个立即执行函数，相当于这5个函数体在声明后会立即执行，进而形成了5个闭包
        (function(j) { //j是形参
            arr[j] = function() {
                console.log('NO.' + j);
            }
        })(i); //i是实参

        //这个循环相当于
        /*
        (function(0){
            arr[0] = function(){
                console.log('NO.' + 0);
            }
        })(0);
        .
        .
        .
        (function(4){
            arr[4] = function(){
                console.log('NO.' + 4);
            }
        })(4);
        */

    }
    return arr; //返回数组（数组是5个函数体）
}
var MyArr = test();
for (var i = 0; i < 5; i++) {
    MyArr[i]();
}

原因：因为立即函数的存在，相当于每次立即函数都会生成一个闭包，闭包存储了j的值，而非i的值（这样闭包就不会去读取外部作用域内的i，而是读取自己作用域内保存的j）
```

------

## 递归

### 定义

函数自己调用自己，就叫递归

### 特点

1、递归如果没有写好结束条件，将会无限循环，一直调用

2、递归调用比较占内存，效率比较低

3、使用递归前需要理解其通项（特点）	如：斐波拉契数（1，1，2，3，5，8...），其通项就是`fn(n-2) + fn(n-1)`

例：

```js
求阶乘
function DG(n) {
    if (n == 0 || n == 1)
        return 1;
    return n * DG(n - 1);	//这个就是通项
}

console.log(DG(5));

/*  n:5 5*DG(5-1)   
    DG(4)   4*DG(3)
    DG(3)   3*DG(2)
    DG(2)   2*DG(1)
    DG(1)   1

    即：5*4*3*2*1
*/
```

------

## 内置对象

### String对象

new String ( ' xxx ' )

#### charAt (  )

> 语法：`str.charAt (index)`
>
> 作用：从一个字符串中返回指定的字符
>
> 参数：index：一个介于0 和字符串长度减1之间的整数。	（ 如果没有提供索引，charAt() 将使用0。）

例：

```js
let str = 'wangze'
console.log(str.charAt(4));     
//结果：z
```

#### charCodeAt ( )

> 语法：`str.charCodeAt (index)`
>
> 作用：判断用户的按键，返回ASCII码
>
> 参数：index：一个介于0 和字符串长度减1之间的整数。	（ 如果index不是一个数，则默认0。）

例：

```js
let str = 'wangze';
console.log(str.charCodeAt(0));     
//结果：119
```

#### 拼接

##### 使用 " + " 拼接

```js
let str1 = 'abc';
let str2 = 'def';
console.log(str1 + str2);
//结果：'abcdef'
```

##### concat( )

`concat` 方法将一个或多个字符串与原字符串连接合并，形成一个**新的字符串并返回**。	数组也同样适用

```js
let str1 = 'abc';
let str2 = 'def';
console.log(str1.concat(str2));
//结果：'abcdef'
```

#### fromCharCode ( )

> 语法：`String.fromCharCode (num)`
>
> 与charCodeAt相反
>
> 参数：num可以是一个数字，也可是多个数字，不过需要逗号隔开
>
> 注：该方法返回一个字符串，而不是一个 `String`对象。

例：

```js
console.log(String.fromCharCode(97));	// a
let str = 'wangze';
console.log(String.fromCharCode(str.charCodeAt(3)));	// g
```

#### substring ( )

> 作用：截取字符串
>
> 语法：`str.substring (indexStart  [, indexEnd])`
>
> 参数：indexStart：需要截取的第一个字符的索引，该索引位置的字符作为返回的字符串的首字母。
>
> ​		   indexEnd：可选。一个 0 到字符串长度之间的整数，以该数字为索引的字符不包含在截取的字符串内。（不写默认截取							   起始位置之后的全部）
>
> 返回：一个新的数组（不影响原数组）
>
> 注：左闭右开，即截取包含起始位置不包含结束位置
>
> 如：
>
> ```js
> let str = 'wangze';
> console.log(str.substring(4));
> //结果：'ze'
> ```

#### slice ( )

> 语法：str.slice ( beginIndex  [, endIndex] )
>
> 作用：提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串
>
> 参数：beginIndex：起始位置（可以为负数，负数时做`Length + beginIndex` 看待）
>
> ​		   endIndex：可选。结束位置如果省略该参数，`slice()` 会一直提取到字符串末尾。
>
> 左闭右开
>
> 注：该方法数组也适用

例：

```js
var aa = "JavaScript百炼成仙";
console.log(aa.slice(1, 4));
//结果：ava
```

#### str [ ]

> 语法：`str[index]`
>
> 作用：h5新增，与charAt相同	用法与数字类似

例：

```js
let str = 'wangze'
console.log(str[4]);     //结果：z
//遍历所有字符
for (let i = 0; i < str.length; i++) {
    console.log(str[i]);
}
```

#### split ( )

> 语法：`str.split ([separator  [, limit]])`
>
> 作用：使用指定的分隔符字符串将一个String对象分割成子字符串数组（即：以指定字符做参考将字符串切成一个数组）
>
> 参数：separator：指定表示每个拆分应发生的点的字符串（字符串或者正则表达式）
> 							  也可以理解为，字符串以separator作为参考，并切片
>
> ​		   limit：一个整数，限定返回的分割片段数量，默认返回数组最大的长度。（即不管最后会切多少个片，设置1，就返回1个切片）

例：

```js
var aa = "JavaScript百炼成仙";
console.log(aa.split('')); 				//结果：['J','a','v'........]
console.log(aa.split('JavaScript')); 	 //结果：['','百炼成仙']
// 限定分割的长度
console.log(aa.split(''), 1); 			//结果：['J']
```

又例：

```js
var str = '小红=99 小白=100 小黄=70 小黑=66 小绿=88';
console.log(aa.split(' '));	//表示以空格作为切片的依据/参考
//结果：[ '小红=99', '小白=100', '小黄=70', '小黑=66', '小绿=88' ]
```

#### match ( )

> 语法：`str.match(regexp)`
>
> 作用：检索返回一个字符串匹配正则表达式的结果	即：用于检测字符串中是否包含匹配模式
>
> 参数：regexp：正则表达式
>
> 返回：找得到返回相数组，数组含对应的字符；否则为null

```ts
var aa = "JavaScript百炼成仙";
console.log(aa.match(/ava/));
//结果：[ 'ava', index: 1, input: 'JavaScript百炼成仙', groups: undefined ]

console.log(aa.match(/ava/g));
//结果：[ 'ava' ]
```

#### replace( )

> 语法：`str.replace(substr, newSubStr)`
>
> 作用：将字符串中的部分值替换为新的值
>
> 参数：substr：将被替换的值（可以是正则）			若找不到substr，则不做替换
>
> ​		   newSubStr：替换后的值
>
> 返回：不改变原数组，返回一个新数组

### Array对象

new Array( )	或	arr = [1 , 2 , 3 , 'wz'...]

#### new Array ( )  创建

> `var arr1 = new Array();`       	  创建一个空数组
>
> `var arr2 = new Array(2);`           这个2表示数组的长度为2，里面有两个空的数组元素	[ empty*2 ]
>
> `var arr3 = new Array(2 , 3);`    等价于[ 2 , 3 ]   这时候存了2，3两个元素的数组

#### push ( )

> 语法：`arr.push(element1, ..., elementN)`
>
> 作用：将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

#### pop ( )

> 语法：`arr.pop()`
>
> 作用：从数组中删除最后一个元素，并返回该元素的值。(**该方法修改原有数组**)

#### unshift ( )

> 语法：`arr.unshift(element1, ..., elementN)`
>
> 作用：将一个或多个元素添加到数组的**开头**，并返回该数组的新长度(**该方法修改原有数组**)

#### shift ( )

> 语法：`arr.shift()`
>
> 作用：从数组中删除**第一个**元素，并返回该元素的值。(**该方法修改原有数组**)

#### splice ( )

> 语法：`arr.splice(index, num, info)`
>
> 作用：通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。(**该方法修改原有数组**)
>
> 参数：index : 从数组的第几个项开始（从1开始）
>
> ​		   num : 删除几个数组项 , 为0 不做删除		非必须
>
> ​		   info: 从删除的位置开始添加数组项			 非必须

例：

```js
例一：
	const months = ['Jan', 'March', 'April', 'June'];
	months.splice(1, 0, 'Feb');
	console.log(months);
	// 结果： Array ["Jan", "Feb", "March", "April", "June"]

	months.splice(4, 1, 'May');
	console.log(months);
    // 结果： Array ["Jan", "Feb", "March", "April", "May"]
-------------------------------------------------------------------------------------
例二：从倒数第 2 位开始删除 1 个元素
	var myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];
	var removed = myFish.splice(-2, 1);
	// 运算后的 myFish: ["angel", "clown", "sturgeon"]
	// 被删除的元素: ["mandarin"]
```

#### sort ( )

> 语法：arr.sort([compareFunction])
>
> 作用：对数组的元素进行排序

例：对数字数组

```js
// 从小到大排序
var arr = [11,44,33,22,66,55];
	arr.sort(function(a,b){
	// 从小到大
	return a - b;
});
console.log("升序:",arr);

// 从大到小排序
arr.sort(function (a,b) {
	return b - a;
})
console.log("降序:",arr);
```

#### join ( )

> 语法：`arr.join([separator])`
>
> 作用：将一个数组的所有元素连接成一个字符串并返回这个字符串。
>
> 参数：separator：可选，指定一个字符串来分隔数组的每个元素
>
> 如：
>
> ```js
> let arr = ['amd', 'yes'];
> console.log(arr.join());		//结果：amd,yes
> console.log(arr.join(''));		//结果：amdyes
> console.log(arr.join('~'));		//结果：amd~yes
> ```

#### reverse ( )

颠倒数组元素的顺序，只用于数组，不用于字符串

把数组转换为字符串并反转：`arr.reverse().join('~')`

#### indexOf ( )

> 语法：`arr.indexOf(searchElement [, fromIndex])`
>
> 作用：返回在数组中可以找到一个给定元素的第一个索引（从0开始）；如果不存在，则返回-1。
>
> 参数：searchElement：要查找的元素
>
> ​		   fromIndex：可选，开始查找的位置。
>
> 注：**字符串和数组对象都适用**

例：

```js
var arr = [ 'blue' , 'white' , 'pink' , 'green' ];
console.log( arr.indexOf('blue') )
//结果：0	

'canal'.lastIndexOf('a');	// 结果：3
```

#### lastIndexOf ( )

与 indexOf ( ) 一样，但是是从后开始检索，不过下标仍从左往右

#### includes( )

es2016新增的

> 语法：`arr.includes(searchElement)`
>
> 参数：arr：需要被索引的数组
>
> ​		   searchElement：要查找的元素
>
> 返回值：一个Boolean，找到为true，否则为false

### Math对象

**使用以下不需要先new一下**

Math.max ( ) ;     取最大值（不能运用在数组中，因为他会先将括号内的隐式转换为数字）

Math.min ( ) ;      取最小值（不能运用在数组中）

Math.abs ( ) ;      取绝对值

Math.floor ( ) ;     表示小于或等于指定数字的最大整数的数字

Math.ceil ( ) ;       返回大于或等于一个给定数字的最小整数

Math.pow ( ) ;      次幂，如`Math.pow(2,7)` 表示2的7次方

Math.round ( ) ;    给定数值的四舍五入（注：`Math.floor( -1.5 )`得-1；`Math.floor( -1.8 )`得-2）

### Date对象

new Date( )

#### Date的括号内没有参数

`var myDate = new Date( ) ;`	返回系统当前时间，非北京时间

#### Date的括号内有参数

表示生成参数的Date对象

数字型：2021,4,23

字符型：' 2021-4-23 15:35:30 ' 或者 ' 2021/4/23 15:35:30 '   		**年月份和时分必须有个空格**

#### 日期的格式化

年：例：`var myDate = new Date()`

​    	`myDate.getFullYear()`

月：例：`myDate.getMonth();`  //返回月份小一个月

​         //因此常写成`myDate.getMonth() + 1`

日：例：`myDate.getDate()`

周：例：`myDate.getDay()`                //周一返回1，周六返回6，周日返回0

时：例：`myDate.getHours()`

分：例：`myDate.getMinutes()`

秒：例：`myDate.getSeconds()`

#### 获得Date总的毫秒数（时间戳）

作用：用于倒计时计算

获得的不是当前毫秒数，而是距离1970年1月1日共过了多少毫秒

例：var inputTIme = new Date('2022.2.1 00:00:00')

#### 秒与天、时、分的转换

d = parseInt(总秒数/60/60/24)       //计算天数

h = parseInt(总秒数/60/60%24)       //计算小时数

m = parseInt(总秒数/60%60)         //计算分钟数

s = parseInt(总秒数%60)         //计算当前秒数

------

