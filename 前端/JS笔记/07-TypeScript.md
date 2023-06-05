# tsconfig.json

参考：https://www.wenjiangs.com/article/immsdbbt.html

## 简介

`tsconfig.json` 是 TypeScript 编译器的配置文件，用于指定编译 TypeScript 代码时的编译选项和编译目标等信息。通过修改该文件，可以定制 TypeScript 编译器的行为，例如指定编译目标、启用或禁用特定的语言特性、设置代码检查规则等。

# d.ts / env.d.ts

目的：ts无法解析我们的vue结尾的文件，配置该文件后，可以让ts成功解析vue文件

目录：将该文件放置在src路径下

```ts
/// <reference types="vite/client" />
 
declare module '*.vue' {
	import { DefineComponent } from 'vue';
	// eslint-disable-next-line @typescript-eslint/no-explicit-any, @typescript-eslint/ban-types
	const component: DefineComponent<{}, {}, any>;
	export default component;
}
 
// 环境变量 TypeScript的智能提示
interface ImportMetaEnv {
	VITE_APP_TITLE: string;
	VITE_APP_PORT: string;
	VITE_APP_BASE_API: string;
}
 
interface ImportMeta {
	readonly env: ImportMetaEnv;
}
```



# TypeScript

## 安装

渐进式，静态类型编程语言

优势：

> 1、规避大量低级错误，避免时间浪费
>
> 2、减少多人协作项目的成本，大型项目友好
>
> 3、良好的代码提示

注：ts代码无法使用node编译为js（只有js代码才能在浏览器和node中执行）

解决：

1、typescript编译器

> 安装：npm install typescript
>
> 编译：npx tsc xxx.ts	（如果typescript是全局包，则可以省去npx）		（编译的意思：将ts编译为js）
>
> 缺点：
>
> > 1、当变量类型改变时，虽报错，但仍能编译成功
> >
> > 2、只编译成js，不能直接执行ts文件（即，其中的打印等操作不会显示）

2、typescript-node

> 安装：npm install typescript-node
>
> 使用：npx ts-node xxx.ts
>
> 优点：一步完成编译+执行

## 类型

数字型：num

字符串型：string

布尔类型：boolean

null：null

未定义：undefined

symbol：symbol

bigint：bigint

任意类型：any

空值：void

未知：unknown

## 引用类型的使用

在ts中，引用类型更加细化，每个对象都有单独的语法

### 类型别名

当多个变量都是同一种数据类型时，如果每个都声明，太麻烦了，特别是联合类型

> 语法：使用关键字type
>
> ```typescript
> type XXX = (number | string)[];		//XXX是自定义的类型别名
> let arr1: XXX = [1, 'a'];
> let arr2: XXX = ['b', 2];
> ```
>
> 在基本类型的声明中都适用，同样对对象也适用

### 数组

> 语法：推荐使用第一行的写法
>
> | 数组中只能出现数字                   | 数组只能出现字符串                    | 数组中既有数字又有字符串                |
> | ------------------------------------ | ------------------------------------- | --------------------------------------- |
> | `let arr: number[] = [1, 2, 3]`      | `let arr: string[] = ['a', 'b']`      | `let arr: (number | string) = [1, 'a']` |
> | `let arr: Array<number> = [1, 2, 3]` | `let arr: Array<string> = ['a', 'b']` | 无                                      |
>
> 注：| 在TS中叫联合类型，只有单竖线，而非双
>
> 以此类推，可以加布尔类型：`arr: (number | boolean | null)[]`
> 如果少了括号：`arr: number | string[]`，则代表arr只能是 数字型（`arr=123`） 或 字符串型数组(`arr=['a']`)													

#### 元组

> 定义：元组类型是特殊的数组，其**确切的知道包含了多少个元素，以及特定索引对应的类型**
>
> 语法：`let arr: [number, number] = [123, 465]`
> 意义：此时的数组有且只有两个数，且必须为number类型

#### 数组对象

表示数组里面的元素为对象

> 语法：
>
> ```ts
> interface type {
>  a: string,
>  b: number
> }
> 
> let arr = <type[]>[
>  {
>      a: 'aa',
>      b: 123,
>  }
> ] 
> 
> // 或者
> 
> interface type {
>     a: string;
>     b: number;
> }[];
> let arr: type = [{
>     a: 'aa',
>     b: 123,
> }]
> ```
>

### 函数

> 定义：指函数的参数和返回值的类型
>
> 语法：
>
> | 单独指定参数、返回值的类型                                   | 同时指定参数、返回值的类型                                   |
> | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | `function add(num1: number, num2: number): number { return 1 }` | 无                                                           |
> | `let add = (num1: number, num2: number): number => { return 1 }` | `let add: (num1: number, num2: number) => number = (num1, num2) => { return 1 }` |
>
> **注**：如果函数的返回值进行了类型声明，则必须有return；如果没return，则应声明为 void 或 any

#### 可选参数

> 原因：使用函数时，参数有时是可传值，也可不传，此时就需要要用到可选参数
>
> 语法：`function xx(变量1?: number, 变量2?: number) {}`
> 意思为：变量1是可选的，类型为number；变量2是可选的，类型也为number
>
> 如：
>
> ```ts
> function fn(uname: string, name?: string): void {
>         console.log(uname + name);
> }
> 
> fn('w');
> fn('w','z');		//这两种都不会报错
> fn();				//此时会报错，因为至少传一个实参
> ```

### 默认值设置

> 作用：给函数的形参设置默认值，当调用函数时，没有给该形参传值时，形参自动使用默认值
>
> 语法：`function (变量1: any, 变量2 = 'abc') {}`
>
> 如：
>
> ```ts
> const fn = (val: string | number | undefined | boolean | null, unit = "") => {
>     let str = unit + "123";
>     console.log(typeof str);
> };
> 
> fn(132);				//此时的unit为""
> ```
>
> 

### 对象

> 对象由属性和方法组成，需要对属性和方法的返回值分别进行类型声明
>
> 例一：
>
> ```ts
> let obj: {name: string; age: number; hobby(myHobby: string): void} = {		//一行写完
>    	name: 'wz',
>    	age: 18,
>    	hobby(myHobby) {
>    	    console.log(this.name + ' love ' + myHobby);
>    	}
> }
> ```
>
> 例二：
>
> ```ts
> let obj: {				//多行写
>   	name: string;
>   	age: number;
>   	hobby: (myHobby: string) => void;			//方法也可以用箭头函数写
> } = {
>   	name: "wz",
>   	age: 18,
>   	hobby(myHobby) {
>    		console.log(this.name + "love" + myHobby);
>   	},
> };
> 
> console.log(obj);
> 
> ```
>
> **注**：
>
> > 1、类型声明必须用`{}`包裹
> >
> > 2、一行里面的属性和方法用`; 或 ,`隔开，见例一；多行则可省略，见例二
> >
> > 3、方法里面有形参，也需要注意形参的类型声明

#### 可选参数

类似函数的可以选参数，都用 ？来表示

如：

```ts
let obj:{
    name?: string
    age?: number
}
```

#### 接口

> 作用：当一个对象类型被多次使用时，可以使用接口（interface）来描述，达到复用的目的
>
> 语法：
>
> ```ts
> interface XXX {
>         age: number
>         name: string
> }
> 
> let obj: XXX = {
>         age: 18,
>         name: 'wz'
> }
> ```
>
> 

#### 接口的继承

接口和接口间，可能存在相同的属性和方法，可以将相同的抽离出来，通过继承来实现复用

如：

```ts
interface A { x: number; y: number; }
interface B { x: number; y: number; z: number }

//继承后
interface A { x: number; y: number; }
interface B extends A { z: number }		//如果B没有自己的属性，则是：interface B extends A {}

//使用
let obj: B = {x: 1, y: 2, z: 3}
```

## 类型推论

定义：在TS中，某些没有明确指出类型的地方，TS的类型推论机制就会帮助提供类型，因此某些地方可以不写类型声明

常见的类型推论场景：

> 1、声明变量并初始化时												如 let a = 123，此时的a就是number类型		
>
> 2、决定函数返回值时（能够推导处return的类型）	  如 `function fn() { return 123 }`，此时的fn返回类型就是number

如果声明变量，但没初始化赋值，也没声明类型，则该变量可以随时变为任意类型

## 类型断言

虽然TS能够提示变量类型，但有可能这个类型太宽泛，导致无法知道具体的属性和方法

如：

> dom类型	
>
> `let app = document.querySelector("#app")`
>
> 此时app的类型只是一个Element，而无法知道其是div或a标签等
>
> 如果是a标签，那么使用 app.href 时，ts会提示错误，但实际却能正常编译

解决：使用类型断言，指定更加具体的类型

> 语法：
>
> > 1、as关键字		
> >
> > 如：`let app = document.querySelector("#app") as HTMLAnchorElement`
> >
> > 2、**<> 语法**
> >
> > 如：`let app = <HTMLAnchorElement>document.querySelector("#app")`

## 字面量类型

> 定义：某个特定的值可以作为TS中的类型
>
> 如：
>
> ```ts
> let str1 = "Hello TS";		//str1的类型为string
> const str2 = "Hello TS";	//str2的类型为 "Hello TS"	因为const定义的是常量，值是不会发生变化的，所以字符串作为了str2的类型，此处的"Hello TS"就是字面量类型
> ```
>
> 因此，可以直接给变量赋字面量类型：
>
> ```ts
> let age: 18 = 19;	//此时就会报错
> ```
>
> 使用场景：明确知道变量的可选值（比如 'up' | 'down'）

### 枚举

> 作用：定义一组命名常量（字面量）
>
> 语法：`enum XXX { 1, xx }`
>
> 使用：`let str: XXX`		此时str的值只能为 1 或  xx
> 访问枚举成员：`XXX.1`	此时就可以拿到 1 了
> 即：
>
> ```ts
> str = XXX.1;		//此时打印str，结果为0		index=0
> str = XXX.xx;		//此时打印str，结果为1		index=1
> ```
> 
>注：
> 
>> 1、不能直接str = xx
> >
> > 2、不能直接使用 XXX.xx ，更不能 XXX.xx = 123		（因为XXX是只读）
> > 但是可以在枚举初始化的时候赋值：`enum XXX { 1='1', xx=2 }`
> > 如果赋值是数字，当第一个赋值，后续的不赋值，则后续的默认自增长

## typeof 运算符

如果不是ts中的类型声明语法，则会将 typeof 识别为 js 中的 typeof

> 作用：在类型上下（就是：前后，就叫类型上下文）文中引用变量或属性的类型
>
> 使用场景：根据已有变量的值，获取该值的类型，以达到简化书写的目的
>
> 如：
>
> ```ts
> let obj = { x: 2, y: "aa" };
> 
> function fn(point: typeof obj) {}
> // 等价于function fn(point:{x:number,y:string}) {}
> let num: obj.x;			// 此时num的类型是number
> ```
>
> ```ts
> const fn = (a: number) => {
>   return 123;
> }
> 
> let a: typeof fn;		// 此时的a的类型是 ()=>number
> let b: typeof fn(1);	// Erro!	不能这样写
> ```
>
> 

# TypeScript高级类型

## class类

TS中的class，不仅提供了class的语法功能，也能作为一种类型存在

> 语法：
>
> ```ts
> class Person {
>     age: number = 18;
>     gender = 'male';
>     tellAge() {
>         console.log('my age is ' + this.age)
>     }
> }
> const p = new Person();		// p的类型是 Person
> p.age = 123;
> p.tellAge();	// 结果是：my age is 123
> ```
>
> 含构造函数的语法：
>
> ```ts
> class Person {
>   age: number;
>   gender = 'male';
>   constructor(age: number, gender: string) {
>     this.age = age;
>     this.gender = gender;
>   }
> }
> const p = new Person(18, '男');		
> console.log(p);	// 结果是：Person { gender: '男', age: 18 }
> ```
>
> 注：
>
> 1、构造函数需要先初始化成员
>
> 2、构造函数的形参需要指定类型注解；构造函数的return不能指定类型，因为没有return（否则就会报错）

### 继承

#### extends 实现 类和类的继承

```ts
class Person{ 
  age: number;
  name: string;
  constructor(name: string, age: number) {
    this.age = age;
    this.name = name;
  }
}

class Singer extends Person {
  sing() {
    console.log(this.name + '会唱歌');
  }
}

let p = new Singer('wz', 18);
p.sing();	// 结果：wz会唱歌
```

#### implements 实现 类和接口的继承

```ts
interface obj {
  name: string;
  age: number;
}

class Person implements obj {
  name = 'wz';
  age = 18;
}
```

注：继承的接口中的属性或方法，必须在类里面声明

### 内成员可见性

使用public、protected、private关键字实现类的属性或方法能否可见/继承

#### public

> public是默认的，表示共有成员可以被任何地方访问
>
> 语法：
>
> ```ts
> class Person {
>     public age: number = 18;
>     gender = 'male';
> }
> ```
>
> 

#### protected

> 表示受保护的，仅能被类和子类中访问，不能在实例对象中访问
>
> 语法：
>
> ```ts
> class Person {
>     protected age: number = 18;
>     gender = 'male';
> }
> ```
>
> 

#### private

> 表示私有的，仅能被当前类访问，不能被子类、实例对象等访问