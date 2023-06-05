# JavaScript高级

## 面向对象

面向过程：

> 定义：就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次调用就可以了。
>
> 核心：强调实现功能的过程
>
> 优点：性能比面向对象高，适合跟硬件联系很紧密的东西
>
> 缺点：没有面向对象易维护、易复用、易扩展

面向对象：

> 定义：是把事务分解成为一个个对象，然后由对象之间分工与合作
>
> 核心：强调的是对象，然后调用对象实现功能
>
> 优点：灵活、代码可复用、容易维护和开发，适合多人合作的大型软件项目；由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统 更加灵活、更加易于维护
>
> 缺点：性能比面向过程低
>
> 特点：
>
> > 封装：隐藏对象的属性和实现细节，仅对外提供公共访问方式。封装可以将变化隔离，便于使用，提高复用性和安全性
> >
> > 继承：通过父子关系引用，子类无需书写父类成员，也能使用父类的成员。继承主要提高代码复用性
> >
> > 多态：父类或接口定义的引用变量可以指向子类或具体实现类的实例对象。多态用来提升程序的拓展性

### 对象

拥有属性和方法的一个集合

特点：

> 1、对象是引用数据类型，操作对象实则是操作其内存地址
>
> 2、存储方法是 键值对（key，value）	key是字符串类型，value是任意类型
>
> 3、对象的属性名可以有特殊符号（如空格、关键字、保留字等），但是**需要给其加引号**，且引用需要用**对象名 [ ' 属性名 ' ]**	如：`iPhone { "12 ++" : '特殊字符' }`  

#### 创建对象

##### 字面量创建

```js
var obj = { 
	uname : ‘张三’ ,
	age : 18 ,
	sex = ‘男’ ,
	//以上是属性
	//以下是方法（方法常用新定义的函数来执行）
	sayHi : function ( ) {
		console.log(‘Hi~’) ;
	}
}
```

注：方法冒号后面跟的是一个匿名函数

##### 利用New Object创建

```js
var obj = new Object ( ) ;
obj.uname = ‘张三’ ;
obj.age = 18 ;	
obj.sex = ‘男’ ;
obj.sayHi = function ( ) {
	console.log( ‘Hi~’ );
} ;
```

##### 工厂模式

如：

```js
function student (name,age) {
    var obj = {}
    obj.nmae = name;
    obj,age = age;
    obj.hoby = function(){
        dosomething();
    }
    return obj;
}
//使用
var stu1 = student('wz',18);
console.log(stu1,stu1.hoby());
```

##### 利用构造函数创建

```js
function 构造函数名 ( ) {
	this.属性 = 值 ;
	this.方法 = function ( ) { }
}
//使用
new 构造函数名 ( ) ;
```

例：

```js
function Star ( uname , age , sex ) {
	this.name = uname ; 
	this.age = age ;
	this.sex = sex ;
	this.sing = function ( sang ) {
		console.log (uname + '的歌曲' + sang ) ;
	}
}	//这里面的name、age、sex都是属性，而unmae、age、sex都是形参
var ldh = new Star( '刘德华' , 18 , '男' ) ; 
var zxy = new Star( '张学友' , 20 , '男' ) ; 
console.log( ldh.name ) ;			//结果是 刘德华
ldh.sing ( '冰雨' ) ;				  //结果是 刘德华的歌曲冰雨
```

注：

> 1、this正常是指向全局windows的，在函数里面时才指向该函数
>
> 2、该函数无需return，默认返回这个对象
>
> 3、构造函数名首字母要大写（惯用写法，便于区分普通函数）
>
> 4、var ldh = new Star ( ) 这个调用函数返回的是一个对象，即console.log( typeof ldh )     结果是object
>
> 5、调用构造函数时，必须使用new
> new有一个隐式三步骤：`var this = {} ; this.xxx ; return this ;`
> 因此也证明了第2点中构造函数无需return，且其this是指向函数本身（如果不new而是直接调用，则this仍指向window）
>
> 6、只要调用了new Star ( ) ,就创建了一个新对象，**所有new一个对象都会执行一遍其构造函数**（如果构造函数内有打印等操作，就会在new的时候触发打印等操作）
>
> 7、属性和方法之前必须添加this

###### 缺点

可能会存在浪费内存的问题，因为如果构造函数里面由共享的属性或方法，每次创建都会开辟地址来储存

###### 解决办法

1、将共享的属性或方法放在构造函数的外面，但要注意同名覆盖问题

例：

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
    this.job = '前端开发';
    this.skill = sing;
}

function sing() {
    console.log(this.name + '会唱歌');
}

var wz = new Student('wz', 18);
var max = new Student('max',16);

wz.skill();	//结果：wz会唱歌

max.age === wz.age 		//结果：false,因此每创建一个实力对象，都会开辟新的job的地址，造成空间浪费
max.skill === wz.skill	//结果：true（是同一个内存地址）
```

1.1、解决命名冲突问题

将方法放到一个新的对象中去

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
    this.job = '前端开发';
    this.skill = pfns.sing;
}
var pfns = {
    function sing() {
    	console.log(this.name + '会唱歌');
	}
}
```

2、通过原型的方式

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype.job = '前端开发';

var wz = new Student('wz', 18);
var max = new Student('max',16);
console.log(wz);		//结果：Student { name: 'wz', age: 18 }	这里面没有job属性，但该实例对象下的prototype对象里面有job属性
console.log(wz.job);	//结果：前端开发

max.age === wz.age 		//结果：false
wz.job === max.job		//结果：true（是同一个内存地址）
```

![js_原型](.\image\js_原型.png)

##### ES6 class类方式创建

类是建立在原型上的（类是使用构造函数 + 原型 创建的对象），且class本质上依旧是函数，而非引用类型（与引用类型相似）

constructor函数：

> 1、类里面有个constructor函数，可以接受传递过来的参数，同时返回实例对象。
>
> 2、constructor函数，只要new了一下，就会自动调用这个函数，如果我们不写这个函数，class类也会自动调用。
>
> 3、类里面的所有函数不用加function
>
> 4、类的共有属性是放在constructor函数里面的
>
> 5、类里面的方法不需要加逗号，以及同第三点，无需加function

如：

```js
class Student {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    skill() {
        console.log('干饭');
    }
    sing() {
        console.log(this.name + '开始唱歌');
    }
};

var wz = new Student('王泽', 18);
console.log(wz);
wz.skill();	//结果：干饭
wz.sing();	//结果：王泽开始唱歌
```

注：创建 一个实例对象不能省略new

#### 增、改属性/方法

> 语法：直接在对象后面加 `Object.XXX = XX`
>
> 注：若对象无XXX属性，则增加改属性 ； 若对象有XXX属性，则更改其值

#### 删除属性

> 语法：`delete Obiject.XXX`
>
> 作用：删除对象的XXX属性

#### 遍历

##### for in 遍历对象

> 语法：`for ( 变量 in 对象 ) { }`

例：

```js
for ( var k in obj ) {
	console.log ( k ) ;		// k是变量，输出得到的是属性名（如name、age、sex）
	console.log ( obj[k] ) ;	//输出得到的是属性值（如张三、18、男）
}
```

##### Object自带方法

结合本章后面笔记  ES5新增方法  -  对象  -  keys、values

> `Object.keys()`		 访问对象的属性名，如 name、age、sex
>
> `Object.value()`		访问对象的属性值，如 刘德华、18、男
>
> 返回：这两个方法返回的都是数组
>

例：

```js
var key = Object.keys(Star);
for(var i = 0; i < key.length; i++){
    console.log(key[i]);		 //返回属性名
    console.log(Star[key[i]]);	 //返回属性值
}
```

### 成员

定义：通常称呼一个类的字段、函数为成员

成员组成：

> 属性：这个类包含的一些键值
>
> 方法：这个类包含的一些函数，用类或者对象来调用该函数
> 方法可以挂到构造函数的 “ 原型 ” 上，也可以直接挂到构造函数上，此时方法又分为：
>
> > 实例方法：由类的实例对象使用
> >
> > 静态方法：也称类方法，由这个类本身调用，挂在构造函数上

如：

```js
var Person = function(name, age) {
    this.name = name;
    this.age = age;
    this.skill = function() {
        console.log('干饭');
    }
};

//创建一个实例对象 wz
var wz = new Person('王泽', 18);

//访问实例成员，只能通过实例化的对象（wz）来访问
console.log(wz);
wz.skill() //结果：干饭

//不可以通过构造函数访问实例成员
console.log(Person.age); //结果：undefined
console.log(Person.skill); //结果：undefined

//静态成员，在构造函数本身添加的成员就是静态成员
Person.sex = '男';
console.log(Person.sex); //结果：男

//静态成员只能通过构造函数来访问，不能通过实例对象来访问		
console.log(wz.sex); //结果：undefined
//就算 Person.sex = '男' 放在 var wz = new Person('王泽', 18)	之后，也是 undefined
```

## 原型

定义：原型是function对象的一个属性，它定义了构造函数制造出来的公共祖先。通过该构造函数构造函数产生的对象,可以继承该原型的属性和方法（引用类型数据：数组、对象）

> 1、所有引用类型都有一个`__proto__`（隐式类型）属性，属性值是一个普通对象（Object）
>
> 2、所有函数都有一个prototype（原型）属性，属性值是一个普通的对象
> 	  即：创建函数都会有一个prototype属性，该属性指向了原型对象
>
> 3、引用类型的`__proto__`属性都指向它 构造函数的 prototype

原型的作用：

> 1、实现数据共享，节省空间
>
> 2、实现继承

构造函数需要原型对象的原因：让多个实例之间通用（/共享）方法使用一个引用，减少空间的占用

### 原型对象的两个属性

> 1、constructor：指向与原型对象关联的构造函数
>
> 2、`__proto__`：指向其构造函数的原型，默认为Object

#### constructor

constructor 主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数

如果修改了原来的原型对象，即给原型对象prototype赋值了一个新的对象，则必须手动利用constructor指回原来的构造函数

##### 给原型对象追加属性 

例：

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype.job = '前端开发';
Student.prototype.sayName = function() {
    console.log('你好' + this.name);
};

var wz = new Student('王泽', 18);

wz.constructor === Student //结果：true（是同一个内存地址）
```

##### 给原型对象赋值一个对象

例：

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype = {
    job: '前端开发',
    sayName: function() {
        console.log('你好' + this.name);
    }
}

var wz = new Student('王泽', 18);

wz.constructor === Student 	//结果：false（因为实例对象wz的prototype对象没有constructor了）
```

解决：

```js
Student.prototype = {
    constructor: Student,
    job: '前端开发',
    sayName: function() {
        console.log('你好' + this.name);
    }
}

wz.constructor === Student	//结果：true
```

#### `__proto__`

每个实例对象都会有一个属性`__proto__` 指向构造函数的prototype 原型对象，之所以我们对象可以使用构造函数prototype 原型对象的属性和方法，就是因为对象有`__proto__` 原型的存在

![js_原型_proto](.\image\js_原型_proto.png)

例：

```js
//原型对象
function Person() {}
console.log(Person.prototype);
/* 结果： constructor: ƒ Person()
		 [[Prototype]]: Object		*/

var Student = function() {}
console.log(Student.prototype);
/* 结果： constructor: ƒ ()
		 [[Prototype]]: Object		*/

//实例化对象
var stu1 = new Student();		//注：实例与构造函数没有直接关系，但和原型对象有
console.log(stu1.prototype);	//结果：undefined
//实例化对象获取原型对象的方法：
console.log(stu1.__proto__);	// stu1.__proto__ === Student.prototype
```

综上，若需要获得对象的原型，实例类型需要用 `XXX.__proto__`；构造函数需要用`XXX.prototype`

### 构造函数、实例、原型 三者的关系

![js_原型_关系](.\image\js_原型_关系.png)

### 原型链

![js_原型链](.\image\js_原型链.png)

原型链查找规则：

> ① 当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性
>
> ② 如果没有就查找它的原型（也就是 `__proto__`指向的 prototype 原型对象）
>
> ③ 如果还没有就查找原型对象的原型（Object的原型对象）
>
> ④ 依此类推一直找到 Object 为止（null）
>
> ⑤ `__proto__`对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线

Object的原型对象：例如daibai这个实例对象，其原型（daibai的原型是`__proto__`）指的是Person的原型对象，而Person原型对象中的constructor指向了Person本身，而Person原型对象也可以视作Object的一个实例化对象（即构造函数Person的`__proto__`指的是Object的原型，Person的constructor指向的是Object）

### instanceof

> 作用：用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上
>
> 语法：stu1 instanceof Student
> 即，检测stu1是否在Student上，是则返回true
>
> 返回：Boolean值

如：

```ts
// 定义构造函数
function C(){}
function D(){}

var o = new C();


o instanceof C; // true，因为 Object.getPrototypeOf(o) === C.prototype


o instanceof D; // false，因为 D.prototype 不在 o 的原型链上

o instanceof Object; // true，因为 Object.prototype.isPrototypeOf(o) 返回 true
C.prototype instanceof Object // true，同上

C.prototype = {};
var o2 = new C();

o2 instanceof C; // true

o instanceof C; // false，C.prototype 指向了一个空对象，o.__proto__ 不是现在的 C.prototype，因此o已经不在当前C的原型链上了

D.prototype = new C(); // 继承
var o3 = new D();
o3 instanceof D; // true
o3 instanceof C; // true 因为 C.prototype 现在在 o3 的原型链上
```

## 继承

定义：子类可以继承父类的一些属性和方法

作用：主要作用是提升代码复用性，让代码更为简洁

分类：

> 接口继承：只继承方法签名，继承者自己来实现具体的方法。
>
> 实现继承：直接继承父类的方法
>
> 注：JS 的函数没有签名，无法实现面向接口编程，JS 的继承主要依赖于原型链实现

实现继承的方法：基于原型链的继承；借用构造函数实现继承；原型式继承；寄生继承

完美继承（没有缺点）：寄生组合继承

### 组合继承

借用构造函数+原型链继承

#### 基于原型链继承

> 核心：原型链对象变成父类的实例，子类就可以调用父类的**方法和属性**
>
> 语法：
>
> ```js
> Son.prototype = new Father();
> Son.prototype.constructor = Son;
> ```

##### 方法

给子元素的原型指向父元素的实例	`Son.prototype = new Father() ; Son.prototype.constructor = Son ;`

例：

```js
//父类
function Father(money) {
    this.surname = '王';
    this.friends = ['张三', '李四'];
    this.money = money;
}

//子类
function Son(name, age) {
    this.name = name;
    this.age = age;
}

//子类继承父类
Son.prototype = new Father();
Son.prototype.constructor = Son;

//子类实例
var son1 = new Son('王泽', 22);
console.log(son1);
```

结果：

![js_继承_原型链继承](.\image\js_继承_原型链继承.png)

例二：

```js
//父类
function Father(name) {
    this.surname = '王';
    this.friends = ['张三', '李四'];
    this.money = name
}

//父类的原型里的方法
Father.prototype.havemoney = function() {
    console.log(`${this.name}有一千万`);	//此时当new实例后this指向Father
}

//子类
function Son(name, age) {
    this.name = name;
    this.age = age
}

//子类继承父类
Son.prototype = new Father();
Son.prototype.constructor = Son;

//子类的原型里的方法    
//注：该代码应该在继承代码之后，因为此时子类的原型是父类的一个实例；如果该代码放在继承的前面，那么在继承后，子类的原型将不再是子类，该代码是原子类原型里面的，父类实例里面没有，因此无法调用该方法
Son.prototype.study = function() {
    console.log(`${this.name}正在学习前端`);
}

//子类实例
var son1 = new Son('王泽', 22);
console.log(son1);
son1.havemoney();
son1.study();
```

结果：

![js_继承_原型链继承2](.\image\js_继承_原型链继承2.png)

##### 缺点

缺点1：多个子类会共享一个原型，导致其中一个子类改变时，其他子类再引用也会改变

> 例：
>
> ```js
> function Father(name) {
> 	this.friends = ['张三', '李四']
> }
> 
> function Son(name, age) {
> 	this.name = name,
> 	this.age = age
> }
> 
> Son.prototype = new Father();
> Son.prototype.constructor = Son;
> 
> var son1 = new Son('wz', 22);
> var son2 = new Son('Max', 18);
> console.log(son1.friends);
> 
> son1.friends.push('王五');
> console.log(son1.friends);
> console.log(son2.friends);
> // 此时 son1.friends === son2.friends （原因：其指向的都是Father原型上的）
> ```
>
> 结果：
>
> ![js_继承_原型链继承_隐患1](.\image\js_继承_原型链继承_隐患1.png)
>

缺点2、不能向父类传递参数（见“例二”结果，里面money为undefined，因为子类传参是给的子类的原型，父类接受不到）

解决办法：借用构造函数

#### 借用构造函数

> 语法：在子级构造函数里面写`Father.call(this)`
>
> ```js
> function Son () {
> 	Father.call(this)
> }
> ```
>
> 核心：子类的构造函数内部调用父类的构造函数，并且传入this指针
>
> 注：**借用构造函数没法继承父类原型上的方法**
> 因此 借用构造函数继承+原型链继承 而成的 组合继承 能全部继承

如：

```ts
function Father(name) {
  this.sing = () => {
    console.log(`${this.name} can sing`);
  }
}

Father.prototype.skill = function () {
  console.log('Father has skills');
}
Father.prototype.uname = 'w';

function Son(name, age) {
  //借用构造函数的继承
  Father.call(this);
  this.name = name,
  this.age = age
}

var son1 = new Son('wz', 22);
var son2 = new Son('Max', 18);


son1.sing();
console.log(son1.uname);	//undefined，因为单个借用构造函数无法继承父类的原型
son1.skill();	//会报错，因为单个借用构造函数无法继承父类的原型
```

结果：

![js_继承_借用构造函数继承_result](.\image\js_继承_借用构造函数继承_result.png)

##### call()、apply()

> 通过call()、apply()方法
>
> > call()：可以修改this的指向，括号内可以带参数
> >
> > apply()：可以修改this的指向，括号内可以带参数，且参数必须是数组形式，多个参数写在一个数组内
> >
> > 两个方法没有太大区别，唯一区别是call接受的是参数列表，apply接受的是一个参数数组
>>
> > 注：调用call()、apply()都会去执行一次，如 `fn.apply(this,arguments)`，此时会执行一次fn函数
>

##### 目的

解决基于原型链继承的缺点

###### 解决子类共享问题

缺点一的解决：Son继承了Father的属性，并且不会出现子类共享的隐患

例：

```js
function Father(name) {
  this.friends = ['张三', '李四']
}

function Son(name, age) {
  //借用构造函数的继承
  Father.call(this);
  this.name = name,
  this.age = age
}

var son1 = new Son('wz', 22);
var son2 = new Son('Max', 18);
console.log(son1.friends);

son1.friends.push('王五');
console.log(son1.friends);
console.log(son2.friends);
```

结果：

![js_继承_借用构造函数_解决子类共享问题](.\image\js_继承_借用构造函数_解决子类共享问题.png)

###### 解决子类传参给父类问题

缺点二的解决：解决原型链继承不能向父类传递参数的问题：`call(this, 参数名)`或`apply(this, [参数名])`

例：

```js
function Father(name) {
    this.name = name;
    console.log('我的名字是' + this.name);
}

function Son(name, age) {
    Father.call(this, name);
    //Father.apply(this, [name])
    this.name = name;
    this.age = age
}

var son1 = new Son('wz', 22);		//结果会打印	我的名字是wz
var son2 = new Son('Max', 18);		//结果会打印	我的名字是Max
```

##### 借用构造函数继承的缺点

缺点3：无论什么情况下，都会多调用一次父类构造函数（借用构造函数继承那段代码会调用一次：`Father.call(this);`）
因此，如果父类有log等操作，则会多执行一次

如：

```ts
function Father(name) {
  this.friends = ['张三', '李四']
  console.log('I am Father');
}

function Son(name, age) {
  //借用构造函数的继承
  Father.call(this);
  this.name = name,
  this.age = age
}

var son1 = new Son('wz', 22);	// 会打印 I am Father
var son2 = new Son('Max', 18);	// 会打印 I am Father
```

解决：利用寄生式组合，免去了new Father这一步骤

#### 借用构造函数+基于原型链继承

```ts
function Father(name) {
  this.friends = ['张三', '李四']
  this.sing = () => {
    console.log(`${this.name} can sing`);
  }
}

Father.prototype.skill = function () {
  console.log('Father has skills');
}
Father.prototype.uname = 'w';

function Son(name, age) {
  //借用构造函数的继承
  Father.call(this);
  this.name = name,
    this.age = age
}

Son.prototype = new Father();
Son.prototype.constructor = Son;

var son1 = new Son('wz', 22);
var son2 = new Son('Max', 18);

son1.sing();				// wz can sing
console.log(son1.uname);	 // w
son1.skill();				// Father has skills
```

### 寄生组合继承

寄生组合继承 = 原型式继承 + 寄生式继承

以下笔记，寄生式继承就是寄生组合继承（因为会包含Object.create）

#### 原型式继承

其思路与基于原型链继承不完全一样

> 核心：需要创建一个临时的构造函数，并将父类的对象作为构造函数的原型，返回一个新对象
>   		   其他继承都需要有一个子类构造函数，而原型式继承会新建一个对象
>
> 语法：
>
> ```js
> function inherit(obj) {
>    	//临时构造函数
>    	function Fn() {}
>    	Fn.prototype = obj;
>    	Fn.prototype.constructor = Fn
>    	return new Fn();
> }
> //或
> function inherit(obj){ 
>    	return Object.create(obj) 
> }
> //或
> let son = Object.create(Father)
> ```
>
> 目的：1、解决组合继承情况下多调用一次父类构造函数的问题
>
> ​			2、继承属性

例：

```js
//原型式继承

//参数是父类
function inherit(obj) {
    //临时构造函数
    function Fn() {
        this.uname = '王'
    }
    Fn.prototype = obj;
    Fn.prototype.constructor = Fn
    return new Fn();
}
//上诉代码可以利用es5新增方法：Object.create()
//即：function inherit(obj){ return Object.create(obj) }

var Father = {
	name: '我的名字是Faher',
	friends: ['张三', '李四', '王五'],
	hobby: function() {
		console.log(this.name + '爱唱歌');
    }
}
    //注：父类不能使用构造函数

var son1 = inherit(Father);
var son2 = inherit(Father);

console.log(son1);
console.log(son1.friends);
console.log('调用子类原有的属性或方法：', son1.uname); //会打印子类的属性

//调用父类的方法
son1.hobby(); //结果：hobby的this指向Father，因此能调用Father里面的属性name

Son.friends.push('赵六');
console.log('新朋友添加后：', son1.friends);
console.log('新朋友添加后：', son2.friends);
```

结果：

![js_继承_寄生组合_result](.\image\js_继承_寄生组合_result.png)

##### 缺点

缺点一：

> 父类的属性与方法，对于子类都是共享的，因此会出现缺点一的问题
>

解决办法

利用寄生式继承

#### 寄生式继承

> 1、利用构造函数来继承属性
>
> 2、利用原型链混成形式继承方法
>
> 语法：
>
> ```js
> function Son () {
> 	Father.call(this)
> }
> 
> Son.prototype = Object.create(Father.prototype)
> Son.prototype.constructor = Son;
> ```
>
> 作用：继承属性和方法
>
> 缺点：因为加入了call、apply，仍然会执行一遍Father

对比组合继承，少了一个new Father一个步骤（从 `Son.prototype = new Father`变成了 `Son.prototype = Object.create(Father.prototype)`）

如：

```js
//父类
function Father(name) {
    this.surname = '王';
    this.friends = ['张三', '李四'];
    this.money = name
}

//父类的原型里的方法
Father.prototype.havemoney = function() {
    console.log(`${this.name}有一千万`);
}

//子类
function Son(name, age) {
    Father.call(this, name)
    this.name = name;
    this.age = age
}

//子类继承父类
Son.prototype = Object.create(Father.prototype)
Son.prototype.constructor = Son;

Son.prototype.study = function() {
    console.log(`${this.name}正在学习前端`);
}

//子类实例
var son1 = new Son('wz', 22);		
var son2 = new Son('Max', 18);	
console.log(son1);
console.log(son2);
son1.havemoney();
son1.friends.push('赵六');
console.log('新朋友添加后：', son1.friends);	//结果：['张三', '李四', '赵六']
console.log('新朋友添加后：', son2.friends);	//结果：['张三', '李四']
```

结果：

![js_继承_寄生式继承](.\image\js_继承_寄生式继承.png)

------

## ES5新增方法

### 包装类

JS的数据类型分为：

> 基本数据类型：undefined、null、boolean、number、string、symbol
>
> 引用数据类型：object、array、Date

在基本数据类型中，还有三个特殊的基本数据类型，其使用和引用数据类型相似，我们称之为包装类（基本类型包装类）：Boolean、Number、String 

原因：这三个基本数据类型能够调用方法、属性等

如：正常的字符串是没法调用方法的，但是实际却能调用length等方法
原因：JS会隐式通过new String()，来将该字符串进行对象化处理（但其仍是string类型，而非object）

#### 字符串型

##### trim ( ) 

> 语法：`str.trim()`
>
> 作用：会从一个字符串的两端删除空白字符（首尾空格，中间的空格不管）

#### 数字型

##### toFixed ( )

> 语法：`num.toFixed(index)`
>
> 参数：index 表示保留 index 位小数
>
> 如：`num = 10 ; num.toFixed(3)` 	结果：`10.000`
>
> 注：该方法会使num转化为string类型（即`typeof num.toFixed(2)`是  string  ）
>

### 数组

稀疏数组：当数组长度大于其内容，那么该数组就是稀疏数组

为了防止有稀疏数组，因此for循环遍历数组在开发中用的较少
一般用forEach和for in

forEach、map、filter、（some、every	这俩返回值是一个Boolean），这5个方法的参数只有两个

> 参数1：回调函数（回调函数的形参有item，index，arr）
>
> 参数2：this希望指向的地方（用得很少）	
> 箭头函数也能用

#### for in

> 作用：数组有多少长度，就遍历多少次，key是 index 的值
>
> 语法：`for(var key in arr)`
>
> 注：该方法对象也有，因此并非数组独有方法
>

#### keys ( )

> 作用：获取数组的下标，不能单独使用
>
> 语法：`arr.keys()`
>
> 用法：`for(index of arr.keys())`	遍历数组的全部下标，index就是下标（for of 是es6的）
>
> 注：Object的keys()、values()、entries()数组都有，用法都同上（一般都用在for of中）
> 

可以结合Object.keys()看，并结合 ES6笔记 - ES6新增方法 - for of 循环

#### forEach ( )

不会对空数组进行检查

> 作用：数组的遍历（**forEach后会修改原数组**）
>
> 语法：`arr.forEach(function(item, index, arr))`
>
> 参数：
>
> > function：回调函数
> >
> > item：数组元素（ arr[i] ）			可以省略
> >
> > index：数组索引值（下标）		可以省略
> >
> > arr：当前被遍历的数组（数组本身，即arr）	可以省略
> >
> > > 回调函数之后的参数，this指向（几乎不用）
> > >
> > > 正常情况下，forEach的this指向window，但如果加了参数2，则this指向 参数2（参数1是fn）
> > >
> > > 如：`arr.forEach(function(item,index,arr){} , Object)`	此时this就由window改为了Object
> > >
> > > 但如果回调函数是箭头函数，则参数2不起作用（因为箭头函数没有this）
> > > 但我实操是有用的，暂且将他视作有用把。。。
>
> 返回：undefined
>
> 如：
>
> ```ts
> let arr1 = [{ age: 18 }, { age: 20 }, { age: 30 }, { age: 11 }];
> arr1.forEach((item) => {
>   item.age += 10;
> })
> 
> console.log(arr1);	// 结果：[ { age: 28 }, { age: 30 }, { age: 40 }, { age: 21 } ]
> ```

#### map ( )

> 作用：对数组的元素进行修改
>
> 语法：`arr.map(function(item, index, arr))`
>
> 如：
>
> ```ts
> let arr = ['abc', true, 1234, undefined, 'aaa'];
> let arr2 = arr.map((item) => (item += '1'));
> // arr2 是 ['abc1', 'true1', '12341', 'undefined1', 'aaa1']
> ```
>
> 注：
>
> > 回调函数的teturn：
> >
> > 正常情况下，需要配合return使用，最后返回一个新数组
> >
> > 如果不用return，那么就map相当于一个forEach
> > 而forEach用return会返回undefined

```js
const fruits = [
    {name: '苹果',read: 10}, 
    {name: '香蕉',read: 8.8}, 
    {name: '菠萝',read: 5}, 
    {name: '草莓',read: 28.88}
]
let newFurits = fruits.map((item, index, arr) => {
    let obj = {};
    //数据处理
    obj.shop = '*' + item.name;
    obj.price = '￥' + item.read;
    return obj;
})
console.log(newFurits);
//结果：0: {shop: '*苹果', price: '￥10'}
//1: {shop: '*香蕉', price: '￥8.8'}
//2: {shop: '*菠萝', price: '￥5'}
//3: {shop: '*草莓', price: '￥28.88'}

//该map方法引用后，不会对原数组fruit产生影响
```

#### filter ( )

> 作用：检查数组中符合条件的所有元素
>
> 语法：`arr.filter(function(item, index, arr))`
>
> 参数
>
> > function：可以有return，
> >
> > item：数组当前项的值数组当前项的值					可以省略
> >
> > index：数组当前项的索引										 可以省略
> >
> > arr：数组对象本身													可以省略
>
> 返回：一个新的数组
>
> .注：
>
> > 1、不改变原素组，且不会对空数组进行检查
> >
> > 2、回调函数必须有return

例：

```js
var arr = [1, 2, 3, 4, 5, 6];
var newArr = arr.filter(function(val) {
    return val > 3;
})
console.log(newArr);	//结果[ 4, 5, 6 ]
```

#### find ( )

> 作用：查找数组中符合条件的第一个元素
>
> 语法：`arr.find(function(item, index, arr))`
>
> 返回：返回符合条件的**第一个**数组元素（非下标），没有就返回undefined
>
> 注：
>
> > 1、不会对空数组进行检查，不改变元素组（filter是只要满足都返回）
> >
> > 2、回调函数必须有return

例：

```js
var arr = ['3', '2', 3, 4, true, 6];
var newArr = arr.find(function(val) {
    return val === 3;
})
var newArr2 = arr.find(function(val) {
    return val === 7;
})
console.log(newArr);		//结果 3
console.log(newArr2);		//结果 undefined
```

#### some ( )

> 作用：用于检测数组中的元素是否满足指定的条件
> 		   如果有一个条件满足，则表达式为true，且剩余元素不再执行条件判断；如果所有元素都不满足条件，则返回false
>
> 语法：`array.some(function(item ,index ,arr))`
>
> 返回：Boolean值
>
> 注：回调函数必须有return

#### reduce ( )

该方法接收的参数与上述不同

> 作用：从左往右对数组进行处理（常用于数组求和、阶乘、数组去重）
>
> 语法：`arr.reduce(function(prev, cur, index, arr ) {} ，参数2)`
>
> 参数1：
>
> > prev：上次运算的结果
> >
> > cur：当前的值
> >
> > index：下标
> >
> > arr：原数组
> >
> > 参数2：this指向
>
> 注：回调函数必须有return
>

求和：

```js
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let newarr = arr.reduce((prev, cur, index, arr) => {
    return prev + cur;
});
console.log(newarr);
//结果：55
```

去重：

```js
let arr = [1, 5, 6, 10, 2, 1, 3, 6, 8, 7, 2, 5, 7, 9, 10];

//map方法
let newarr = [];
arr.map((cur, index, arr) => {
    if (newarr.indexOf(cur) == -1) {
        newarr.push(cur);
    }
    return newarr;
});
newarr.sort(
    (a, b) => a - b
);
console.log(newarr);		//结果：去重[1, 2, 3,  5, 6, 7, 8, 9, 10]

//reduce方法
let resetarr = arr.reduce((prev, cur) => {
    if (prev.indexOf(cur) == -1)
        prev.push(cur);
    return prev;
}, []);	//使用了参数2，将this指向了数组，从而使prev等形参有了数组的方法
console.log(resetarr.sort(
    (a, b) => a - b
));							//结果：去重[1, 2, 3,  5, 6, 7, 8, 9, 10]
```

#### reduceRight ( )

从右往左计算

其他完全与reduce一样

### 对象

#### 对象的通用方法

| 方法/属性                                    | 描述                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| constructor                                  | 保存当前对象的构造函数                                       |
| hasOwnProperty(propStr)                      | 检测实例对象是否包含该属性，用于检查给定的属性（不会检测原型中的属性） |
| isPrototypeOf(protoObj)                      | 用于测试一个对象是否存在于另一个对象的原型链上               |
| propertyIsEnumerable(propStr)                | 检测传入的参数属性是否能够被 for-in 枚举到。                 |
| toString()                                   | 返回对象的字符串表示                                         |
| valueOf()                                    | 返回对象的字符串、数值或布尔值表示                           |
| getPrototypeOf(obj)                          | 返回 obj 实例对应构造函数的原型                              |
| Object.keys()                                | 返回obj的键（属性名），在数组中返回index下标                 |
| Object.defineProperty(obj, prop, descriptor) | 定义新属性或修改原有的属性（只有Object才有该方法）           |
| Object.defineProperties()                    | 同时定义多个属性的方法（只有Object才有该方法）               |

#### 对象的权限

又称对象的特征

对象的成员在js引擎中有着各式各样的特征，如是否可以被访问、修改，这些特征不能被开发者直接访问

##### 数据属性

作用：深入限制成员：改变成员的特征

> configurable：表示是否可以通过delete删除这个属性，是否可以修改属性的特征，默认是true
>
> Writable：表示是否可以修改属性的值，默认是true
>
> Enumerable：表示是否能通过for in循环返回这一属性，默认是true
>
> value：修改或者新增属性的值，默认是undefined

##### 访问器属性

四个属性

> configurable：表示是否可以通过delete删除这个属性，是否可以修改属性的特征，或者能否把属性修改为访问器属性，默认是true
>
> Writable：表示是否可以修改属性的值，默认是true
>
> get：在读取属性时调用的函数，默认是undefined
>
> set：在写入属性时调用的函数，默认是undefined

get和set两种写法都可以，即	`get:function() {}  ===  get() {}`

注：get函数中，return不能返回其本身prop，否则会一直执行get函数，导致溢出

如：

```js
Object.defineProperty(person, 'name', {	//给person对象新增/修改一个name属性
	get(){
        return this.name;			//会导致一直循环执行get函数
    },
    set(value){
    	this.name = value;			//永远不会执行set函数
	}
})
```

解决办法：

```js
Object.defineProperty(person, 'name', {
	get:function(){					
        return this._name;			//利用中间变量，解决一直循环的问题
    },
    set:function(value){
    	this._name = value;			//将新的值传给_name，进而传给name
	}
})
```

使用set和get的原因：name属性没法进行直接修改时（ 即Writable : false ），可以通过该方法进行修改

##### 权限的更改

如果需要修改这些权限，则要借助ES5中的**defineProperty()**

在执行defineProperty()代码时，其属性默认都为假，但是不执行该方法时，其默认为true
即，调用defineProperty()方法时，需要将其内的configurable等属性设为true，才能进行修改

#### Object.defineProperty()  

##### 数据属性修改

> 语法：Object.defineProperty( obj , prop , descriptor )
>
> 参数
>
> > obj：指定对象，需要修改权限（特征）的对象
> >
> > prop：修改或新增的属性名
> >
> > descriptor：属性的特性（属性描述器）	一般由 ‘ { } ’ 包括
>
> 注：如果使用了defineProperty，但并没写configurable、writable的代码，则其默认为false（即不可删、改等）
>

例：

```js
var person = {};
//让name特征为false
Object.defineProperty(person, 'name', {
    configurable: false, //属性不许被删除或再次修改
    writable: false, //不许修改该属性
    enumerable: false, //不允许遍历
    value: 'wz'
})
console.log(person.name);	 //结果：wz

person.name = 'max';
console.log(person.name); 	//结果：wz				而非max

//没写configurable、writable的代码，则其默认为false
Object.defineProperty(person, 'age', {
    value: 22
})
console.log(person.age);	//结果：22
person.age = 18;
console.log(person);		//结果：22

for (var key in person) {
    console.log(key); //结果：没法遍历
}
```

##### 访问器属性修改

get和set后面必须跟函数

```js
var book = {
    _year: 2004, //年份
    edition: 1 //版本
}

Object.defineProperty(book, 'year', {
    get: function() { // 获取 值--> 只读
        return this._year;		//即：获取到book属性_year信息，而非当前prop year的信息
    },
    set: function(newValue) { //设置值 / 写入值 ---> 只写
        // newValue ---> year（即year就是实参，newValue是形参）
        console.log('new', newValue);

        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
})

book.year = 2005;
console.log(book.edition); //结果：2
```

#### defineProperties()  

用法和defineProperty() 类似

> 语法：Object.defineProperties( obj , props )  
>
> 参数：
>
> > obj：指定对象，需要修改权限（特征）的对象
> >
> > props：修改或新增的属性名（一个或者多个）			一般由 ‘ { } ’ 包括
>
> 作用：直接在对象上定义一个或多个新的属性，或修改多个属性

例：

```js
var person = {};
Object.defineProperties(person,{
    //数据属性
    name:{
    	configurable: false, //属性不许被删除或再次修改
    	writable: false, //不许修改该属性
    	enumerable: false, //不允许遍历
    	value: 'wz'
    },
    sex:{
        configurable: false, //属性不许被删除或再次修改
    	writable: false, //不许修改该属性
    	enumerable: false, //不允许遍历
    	value: '男'
    }
    //访问器属性
    age:{
    	get:function(){					
        	return this._age;			
    	}
    	set:function(value){
    		this._age = value;			
		}	
	}
})
```

#### Object.keys()

> 语法：Object.keys(obj)
>
> 参数：obj：指定的对象
>
> 作用：获取对象的成员
>
> 返回：一个数组（数组的值就是obj的属性 名 ）
>
> 注：该方法只能获取对象中可被枚举的属性（枚举：能被遍历的，原型上属性的就不能遍历）
>

##### 数组中

如：

```js
let fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(Object.keys(fruits));
//结果：[ '0', '1', '2', '3' ]
//获取数组下标
```

##### 对象中

```js
var obj = {
    id: 1,
    pname: '小米',
    price: 1999,
    num: 2000
};
var arr = Object.keys(obj);
console.log(arr);
//结果：[ 'id', 'pname', 'price', 'num' ]
//获取对象属性名
```

#### Object.values()

> 语法：Object.values(obj)
>
> 参数：obj：指定的对象
>
> 作用：获取对象的属性值
>
> 返回：一个数组（数组的值就是obj的属性 值 ）
>
> 注：该方法只能获取对象中可被枚举的属性（枚举：能被遍历的，原型上属性的就不能遍历）
>

如：

```js
let obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(obj));
//结果：[ 'b', 'c', 'a' ]
```

在数组中就是获取数组的值

#### Object.entries()

**ES6新增的**

> 语法：Object.entries(obj)
>
> 参数：obj：指定的对象
>
> 作用：获取对象的属性值
>
> 返回：多个数组（每个数组对应一个 键值对）
>
> 注：该方法只能获取对象中可被枚举的属性（枚举：能被遍历的，原型上属性的就不能遍历）
>

如：

```js
let obj = { 100: 'a', 2: 'b', 7: 'c' };
for (let [key, value] of Object.entries(obj)) {
    console.log([key, value]);
} 
//结果： [ '2', 'b' ]	[ '7', 'c' ]	[ '100', 'a' ]

//这是解构后的
```

#### keys、values、entries总结

> keys：返回属性名（数组就返回`index`下标）
>
> values：返回属性值（数组就返回数组值）
>
> entrie：返回键值对（数组就返回`index`下标和 `arr[index]`值）

#### in方法

> 语法：xxx in obj
>
> 作用：检测 属性或方法 是否在obj中
>
> 返回：一个Boolean值
>

如：

```js
function Son(name) { 
    this.name = name; 
}
var aa = new Son('wz')
console.log('name' in aa);	//结果：true
console.log('toString' in aa);	//结果：true
```

会检测原型链

#### hasOwnProperty() 

> 语法：obj.hasOwnProperty( ' XXX ' ) 
>
> 作用：不会检测原型链中的属性，只会检测到自有属性且是可以枚举的属性

如：

```js
function Son(name) { 
    this.name = name; 
}
var aa = new Son('wz')
console.log(aa.hasOwnProperty('name'));	//结果：true
console.log(aa.hasOwnProperty('toString'));	//结果：false	因为该方法在原型链上
```

#### propertyIsEnumerable()

> 语法：obj.propertyIsEnumerable( ' XXX ' ) 			用法和hasOwnProperty一样
>
> 作用：判断是否可以枚举

#### valueOf()

> 语法：xxx.valueOf()
>
> 返回：指定对象的原始值

例：

```js
// Array：返回数组对象本身
var arr = ["ABC", true, 12, -5];
console.log(arr.valueOf() === arr);   // true

// Number：返回数字值
var num =  15.26540;
console.log(num.valueOf());   // 15.2654

// new一个字符串对象
var str2 = new String("http://www.xyz.com");
// 两者的值相等，但不全等，因为类型不同，前者为string类型，后者为object类型
console.log( str2.valueOf() === str2 );   // false
```

#### create()

语法：`Object.create()`

```js
function inherit(obj) {
    //临时构造函数
    function Fn() {}
    Fn.prototype = obj;
    Fn.prototype.constructor = Fn
    return new Fn();
}
//上诉代码可以利用es5新增方法：Object.create()
//即：
function inherit(obj){ 
    return Object.create(obj) 
}
```

------

## 原生JS实现双向绑定

双向绑定：上面输入了什么内容，下面就显示什么内容

主要方法：

> 1、get 获取值
>
> 2、set 设置值

```html
<body>
    输入的:<input type="text" id="input" />
    <br> 接收的:
    <p></p>

    <script>
        var input = document.getElementById('input');
        var p = document.querySelector('p');

        var data = {
            msg: '这是一个数据'
        }

        Object.defineProperty(data, 'msg', {
            get: function() {
                // 当msg发生改变时，获取input输入框内的内容
                return input.value;
            },
            set: function(newValue) {
                p.innerText = newValue;
                input.value = newValue;
            }
        })

        //  当键盘 抬起 的时候 让 data.msg 获取新的值 ---> 对 data.msg 重新 赋值
        input.onkeyup = function() {
            data.msg = input.value
            console.log(data.msg);
        }
    </script>
</body>
```

## JSON

JSON是一种轻量的数据交互格式

JSON分

> JSON对象		如 { ''name" : "wz" } 
>
> JSON数组		如 [ "1" , 2 , true , { ''name" : "wz" } ]
>
> JSON字符串	如 ' { ''name" : "wz" } '	（外部单引号，里面放对象或者数组）

格式：	关键字	值

> 关键字：又称属性，是一个字符串格式，因此必须使用双引号包括
>
> 值：可以是数字、字符串、Boolean、数组、对象、null
>
> 注：在JSON中不识别function（即没有函数）

### 对象序列化和反序列化

解决JS中对象和JSON格式的不同（JSON要加引号，但是JS不需要）

注：使用序列化和反序列化，进行的是**深拷贝**操作（拷贝了属性和类型，含结构、指针等，相当于创造了一个一模一样的对象）

#### 序列化`JSON.stringify()`

> 作用：把不标准的JSON对象转换成标准的JSON对象
>
> 语法：JSON.stringify ( obj )
>
> 参数： obj：需要转化的对象
>
> 注：序列化后，对象的类型会变为string类型，因此对象的遍历等方法无法再使用
>

#### 反序列化`JSON.parse()`

> 作用：将JSON对象转换为一个对象
>
> 语法：JSON.parse ( obj )
>
> 参数： obj：需要转化的对象
>

## 深浅拷贝

深浅拷贝是针对引用数据类型使用得

基本数据类型是存放在stack（栈）中的；引用数据类型是指针存放在stack中，真实的值是存在堆内存中

### 浅拷贝

#### 拷贝基本数据类型

创建一个新地址，并存放与原始值一样得内容（精确拷贝）

改变值不会影响到原有得值

#### 拷贝引用数据类型

对于引用数据类型，在赋值过程中，赋得是地址，而非堆内存中得数据，因此改变数据会使原数据也改变

即拷贝的是内存地址

#### 代码

对普通对象的拷贝：（拷贝后成一个新的对象，新对象和原对象的**基本数据类型互不影响**，但是**引用数据类型仍会影响**）

###### 基本数据类型互不影响

```js
基本数据类型互不影响
var person = {
        name: '王泽',
        age: 22,
        sex: '男'
}

//浅拷贝，复制的是地址
var wz = person;				//修改wz的属性，会影响到person对象

var max = shallowCopy(person);	 //修改max的属性将不再影响person和wz对象

function shallowCopy(obj) {
    var newObj = {};
    for (var key in obj) { //遍历对象，以复制
        if (obj.hasOwnProperty(key)) { //判断属性能否枚举（即，不复制原型链上的属性）
            newObj[key] = obj[key]
        }
    }
    return newObj;
}
```

###### 引用数据类型仍会影响

```js
引用数据类型仍会影响
var person = {
	name: '王泽',
	wallet: [5, {
	    card1: '身份证',
	    card2: '银行卡'
	}]
}
//浅拷贝，复制的是地址
var wz = person;

var max = shalloCopy(person);
max.wallet[1].card1 = '医保卡';		//结果是，max、person、wz的wallet里面的card1属性都是‘医保卡’

function shalloCopy(obj) {
    var newObj = {};
    for (var key in obj) { //遍历对象，以复制
        if (obj.hasOwnProperty(key)) { //判断属性能否枚举（即，不复制原型链上的属性）
            newObj[key] = obj[key]
        }
    }
    return newObj;
}
```

### 深拷贝

会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。

#### 序列化和反序列化

JSON序列化和反序列化：JSON.stringify()和JSON.parse()

> 原理：相当于创建了新的对象, 而且 会开辟新的栈空间,实现深拷贝
>
> 缺点：
>
> > 1、JSON不能接受函数，因此没法拷贝对象的方法（直接过滤掉function）
> >
> > ```ts
> > const obj = {
> >     date: new Date()
> > }
> > 
> > //序列化、反序列化后，date属性从对象变为了字符串，此时不能再调用obj.date.getTime()等方法
> > ```
> >
> > 2、会丢失undefined类型
> >
> > ```ts
> > const obj = {
> >     a: undefined
> > }
> > const objCopy = JSON.parse(JSON.stringify(obj));
> > // 结果： {}
> > ```
> >
> > 3、当对象中有NaN、Infinity和-Infinity这三种值的时候 --- 会变成null
> >
> > > 1.7976931348623157E+10308 是浮点数的最大上线 显示为Infinity
> > >
> > > -1.7976931348623157E+10308 是浮点数的最小下线 显示为-Infinity
> >
> > 4、当对象循环引用的时候 --会报错
> >
> > ```ts
> > const obj = {
> >     objChild:null
> > }
> > obj.objChild = obj;
> > ```
> >
> > 

#### 递归实现深度克隆（深拷贝）

> 原理：遍历对象、数组，直到里面都是基本数据类型，然后再去复制

代码

```js
function deepCopy(obj) {
    // 判断 obj属性值属于哪种类型（基本数据类型/引用数据类型）
    var objClone = Array.isArray(obj) ? [] : {}; // 判断是数组 还是 对象；是对象则创建一个新对象来clone
    // 判断类型 是不是 对象
    if (obj && typeof obj === 'object') {
        // 遍历对象
        for (var key in obj) {
            //判断属性能否枚举（即，不复制原型链上的属性）
            if (obj.hasOwnProperty(key)) {
                // 判断 obj结构里每一项的值是否存在对象  如果有 就递归复制
                if (obj[key] && typeof obj[key] === "object") {
                    // 继续遍历
                    objClone[key] = deepCopy(obj[key])
                } else {
                    // 如果不是, 属于基本数据类型
                    objClone[key] = obj[key]
                }
            }
        }
    }
    return objClone; // 返回最终克隆的结果
}
```

#### 第三方库lodash中的cloneDeep()方法

其本质就是上面的递归函数

```ts
import lodash from 'lodash';
const newObj = lodash.cloneDeep(obj);
```



总结：递归和lodash是最优的，基本不会有问题出现