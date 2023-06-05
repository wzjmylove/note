# Less

定义：一门向后兼容的CSS扩展语言，也是CSS的预处理语言。

特定：less支持直接使用变量、函数等

## 变量

@arguments，和函数的arguments一样，都是获得全部实参

### 普通参数变量

> 使用位置：在less最低端使用
>
> 语法：`@xxx: xx;`
>
> 使用：
>
> ```less
> @w: 100px;	//必须有分号
> 
> div {
> 	width: @w;
>     	height: @w;
> }
> ```
>
> 注：变量尽量不用数字开头，且不能以符号开头

### 选择器变量

> 语法：`@xxx: .class;`	|	`@xxx: #id;`
>
> 使用：必须用`@{}`将其包裹起来
>
> ```less
> @my-class: .wrap;
> @my-id: #app;
> 
> @{mu-class}{
>     width: 100px;
> }
> 
> @{my-id}{
>     height: 100px;
> }
> ```
>
> 

### 属性变量

> 语法：`@xxx: xx;`
>
> 使用：必须用`@{}`将其包裹起来
>
> ```less
> @bgc: background-color;
> 
> div{
>     @{bgc}: #fff;
> }
> ```
>
> 

### 路径变量

> 语法：`@xxx: xx;`
>
> 使用：必须用`@{}`将其包裹起来
>
> ```less
> @myUrl: '../img/';
> 
> div{
>     background: url('@{myUrl}1.jpg');		//此时的完整路径为：../img/1.jpg
> }
> ```
>
> 

### 声明变量

> 语法：`@xxx: { xx1; xx2; }`
>
> 使用：`@xxx();`
>
> 如：
>
> ```less
> @myDiv{
>     background-color: red;
>     width: 100px;
>     height: 100px;
> }
> 
> #app{
>     @myDiv();
> }
> ```
>
> 

### 变量运算

变量是支持 + - * / 运算的

如：

```less
@w: 100px;

div{
    width: @w - 20;			//注意 + - 符号中间需要有空格隔开；* / 不需要空格
    height: @w -10*2; 		//结果：height是80px
}
```

## 混合

定义：将一组属性从一个规则集包含（或混入）到另一个规则集的方法。
可以理解为继承。

### 复制混合

> 如：
>
> ```less
> .one {
>  background-color: red;
>  width: 100px;
>  height: 100px;
> }
> 
> .two {
>     .one;		//也可以写成	.one()
> }
> //此时类名为two的样式将会和one一模一样
> ```
>
> 

### 继承混合

> 如：
>
> ```less
> .one {
>  color: red;
>  width: 100px;
>  height: 100px;
> }
> 
> .two {
>     .one();
>     color: pink;
> }
> 
> 
> //此时仍会完全复制one的样式，再根据前后顺序，来执行
> //在该处，因为是先混合，因此color为pink
> ```
>
> 

### 有参混合

> ```less
> .one(@a: 100px, @b: deeppink) {
>   width: @a;
>   height: @a;
>   background-color: @b;
> }
> 
> .two {
>   .one();		//结果：不给参数，默认使用形参赋值时的参数
> }
> 
> .three {
>   .one(50px, green);	//结果：是宽高为50px，背景颜色是green
> }
> 
> //注：传参时，必须带单位
> ```
>
> 

## 条件语句

不是if else，而是when

> 语法：`when(判断语句) {}`
>
> 如：
>
> ```less
> #father {
>     .son (@w, @c, @s) when (@w > 100px) {
>         border: @arguments;
>     }
> }
> 
> #app {
>     #father > .son(150px, red, solid);
> }
> 
> //此时 #app的样式是带有border的
> ```
>
> 

### 与 and

> 语法：`when(判断语句1) and (判断语句2) {}`
>
> 如：
>
> ```less
> #father {
>     .son (@w, @c, @s) when (@w > 100px) and (@s == solid) {
>         border: @arguments;
>     }
> }
> 
> #app {
>     #father > .son(150px, red, solid);
> }
> //此时 #app的样式是带有border的
> ```
>
> 

### 非  not

> 语法：`when not (判断语句1) {}`