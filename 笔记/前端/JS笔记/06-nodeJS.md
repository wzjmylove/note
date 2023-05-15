# node

简介：就是一个运行环境，该引擎就是V8引擎，但不同于浏览器，其是JS的后端运行环境

内置API：fs、path、http、quertstring（没有DOM、BOM、AJAX）

## 终端

定义：专为开发人员设计的，用于实现人机交互的一种方式

常用指令：

> cd + 路径 		进入当前磁盘内的文件夹
>
> D:					进入D盘
>
> tab				   自动补全文件名
>
> ↑					  执行上次一操作
>
> esc				  清空当前操作的指令
>
> cls/clear		   清空页面
>
> CTRL + c		 停止服务器运行
>
> nodemon		它能够监听项目文件的变动，当代码被修改后，nodemon 会自动帮我们重启项目

------

## fs文件系统模块

fs模块是node的内置模块，用于操作文件

fs引入方式：`const fs = require('fs')`
只要安装了node，就会有fs模块，引入时，require会自动取查找fs模块

### fs.readFile()		

> 作用：用于读取指定文件中的内容
>
> 语法：`fs.readFile(path , [option] , callback)`
>
> 参数：
>
> > path：字符串，表示文件路径
> >
> > option：字符串，以什么编码格式来读取文件，无默认值		可选
> >
> > callback：文件读取成功后，通过回调函数拿到读取的结果
> >
> > > callback函数里面有两个参数	`(err, result) => {}`	
> > >
> > > err：读取失败时的结果，读取成功时默认为null
> > >
> > > result：读取成功时，文件内的内容，读取失败是未undefined
> > >
> > > 即：读取失败时，err为错误对象，result为undefined；读取成功时，err为null，result为文件内容

例：

```js
const fs = require('fs');
fs.readFile('./code.txt', 'utf8', (err, result) => {
    console.log('err:', err);
    console.log('result', result);
})
```

#### 判断文件是否读取成功

通过判断err的值是否为null即可

例：

```js
const fs = require('fs');
fs.readFile('./code.txt', 'utf8', (err, result) => {
    if (err) {
        return console.log('文件读取失败\n', err.message);
    }
    console.log('文件读取成功，内容是\n', result);
})
```

### fs.writeFile()		

> 作用：用于向指定的文件写入内容
>
> 语法：`fs.writeFile(file , data , [options] , callback)`
>
> 参数：
>
> > path：字符串，表示文件路径	（如果找不到该path下的文件，则新建一个）
> >
> > data：表示要写入的内容
> >
> > options：字符串，以什么编码格式来读取文件，默认为utf8		可选
> >
> > callback：文件读取成功后，通过回调函数拿到读取的结果
> >
> > > callback函数里面有两个参数	`(err, result) => {}`	
> > >
> > > err：读取失败时的结果，读取成功时默认未null
>
> 注：一个文件重复调用writeFile方法，会使新内容全部覆盖旧内容

例：

```js
const fs = require('fs');

fs.writeFile('./file/test2.txt', 'Hello WZ!', 'utf8', (err) => {
    if (err) {
        return console.log('文件写入失败\n', err.message);
    }
    console.log('文件写入成功');
})
```

### 文件路径解决办法

文件路径的问题：

正常的 ./ 和 ../ 是相对路径，在给path赋值相对路径，很容易发生路径动态拼接的问题

路径动态拼接问题：就是在A文件夹里面调用另一个B文件夹下的代码，该代码内的相对路线会取拼接A文件夹的路径，但是该代码的相对路径是相对于B文件夹来说的，因此会报错，该错误就被称为路径动态拼接问题。

#### 绝对路径

给path一个绝对路径

绝对路径中，路径之间用两个斜线分割（因为一个斜线在JS中代表转义的意思）

如：`F:\\学习\\前端培训\\课程练习\\node\\fs\\test.js`

缺点：移植性差，不利于维护

#### __dirname

解决了绝对路径的移植性差问题

node中有个标识符：__dirname

表示当前文件所处的目录(当前JS所处目录)

语法：`__dirname + './xxx/xx.js'`

如：

```js
const fs = require('fs');
fs.readFile( __dirname + '/code.txt', 'utf8', (err, result) => {})
```

## path路径模块

path 模块是 Node.js 官方提供的、用来处理路径的模块

path引入方式：`const fs = require('path')`
只要安装了node，就会有path模块，引入时，require会自动取查找path模块

### path.join()

> 作用：用来将多个路径片段拼接成一个完整的路径字符串
>
> 语法：`path.join([...paths])`
>
> 参数：paths：字符串，路径，可以是多个		可选
>
> 返回：一个拼接之后的字符串
>
> 注：如果出现 ../ ，那么会抵消一次路径

如：

```js
const path = require('path')

const mypath = path.join(`${__dirname}/file/code.js`)
console.log(mypath);
//结果：'f:\学习\前端培训\课程练习\node\path\file\code.js'

const a = path.join('/a', '/b/c', '../', './d', 'e');
console.log(a);
//结果：'\a\b\d\e'
```

与传统 + 拼接字符串的方式表示路径的不同：path.join()方法可以将 ./ 中的 . 给忽略掉，但如果是传统方法，那么将会报错（因为找不到`a/b./c`路径下的文件）

### path.basename()

> 作用：用来从路径字符串中，将文件名解析出来
>
> 语法：`path.basename(path, [ext])`
>
> 参数：
>
> > path：字符串，表示文件路径
> >
> > ext：文件拓展名（如txt、js），一般用来切片删除扩展名	可选参数
>
> 返回：字符串，路径中的最后一部分

如：

```js
const path = require('path');

let fpath = path.join('a', './b', 'c/index.html');
console.log(fpath);
//结果：'a\b\c\index.html'

let fullName = path.basename(fpath);
console.log(fullName);
//结果：'index.html'

let nameWithoutExt = path.basename(fpath, '.html');
console.log(nameWithoutExt);
//结果：'index'
```

### path.extname() 

> 作用：可以获取路径中的扩展名部分
>
> 语法：`path.extname(path,[ext])`
>
> 参数：path：字符串，表示文件路径
>
> 返回：字符串，文件的拓展名（带 **.** ）

如：

```js
const path = require('path');

let fpath = path.join('a', './b', 'c/index.html');
console.log(fpath);
//结果：'a\b\c\index.html'

let fext = path.extname(fpath);
console.log(fext);
//结果：'.html'
```

------

## HTTP模块

> 客户端：在网络节点中，负责消费资源的电脑，叫做客户端
>
> 服务器：负责对外提供网络资源的电脑，叫做服务器
>
> 服务器和客户端的区别：服务器上安装了 web 服务器软件，例如：IIS、Apache 等。同理通过安装这些服务器软件，就能把一台普通的电脑变成一台 web 服务器。

http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。

在 Node.js 中，我们不需要使用 IIS、Apache 等这些第三方 web 服务器软件。因为我们可以基于 Node.js 提供的 http 模块，**通过几行简单的代码，就能轻松的手写一个服务器软件**，从而对外提供 web 服务。

http引入方式：`const http = require('http')`
只要安装了node，就会有http模块，引入时，require会自动取查找http模块

### 创建基本Web服务器

步骤

> 1、导入 http 模块
>
> 2、创建 web 服务器实例：调用 http.createServer() 方法，即可快速创建一个 web 服务器实例
>
> 3、为服务器实例绑定 request 事件（客户端向服务器发送访问请求，就会调用request）
>
> 4、启动服务器：调用服务器实例的 .listen() 方法，即可启动当前的 web 服务器实例

代码：

```js
// 1.导入http模块
const http = require('http');
//2.创建Web服务器实例
const server = http.createServer();
//3.为服务器绑定request事件，监听客户端发过来的请求
server.on('request', (req, res) => {
    console.log('欢迎访问我的服务器');
});
//4.启动服务器
server.listen(8080, () => {
    console.log('服务器运行在这个网址:http://127.0.0.1:8080');
})
```

#### http.createServer()

> 作用：创建 web 服务器实例
>
> 语法：`const server = http.createServer()`

#### server.on()

> 作用：客户端向服务器发送访问请求request，就会调用on方法里面的回调函数
>
> 语法：`server.on('request', function(req,res) {} )`
>
> 参数：
>
> > request：客户端向服务器发送的请求，且请求为reqest；或者是respond
> >
> > req：
> >
> > > 定义：req是与客户端相关的数据或属性
> > >
> > > 常见属性：
> > >
> > > > req.url：客户端访问的地址，非服务器地址，而是ip后面的 / 、/index.html 等
> > > >
> > > > req.method：客户端向服务器发起的method请求类型（如get、post）
> >
> > res：
> >
> > > 作用：向客户端响应一些内容
> > >
> > > 常见方法：
> > >
> > > > res.end(xxx)：向客户端响应xxx内容
> > > >
> > > > res.setHeader('Content-Type', 'text/plain;charset=utf-8')：防止中文乱码

#### server.listen()

> 作用：启动服务器
>
> 语法：`server.listen(端口号, function() {})`
>
> 参数：端口号：服务器的访问端口（默认是80）

------

## 模块化

定义：模块化是指解决一个复杂问题时，自顶向下逐层把系统划分成若干模块的过程。对于整个系统来说，模块是可组合、分解和更换的单元。

编程中的模块化：**遵守固定的规则**，把一个大文件拆成独立并互相依赖的多个小模块

优点：

> 1、提高了代码的复用性
>
> 2、提高了代码的可维护性
>
> 3、可以实现按需加载

Node.js 中根据模块来源的不同，将模块分为了 3 大类，分别是：

> 1、内置模块（内置模块是由 Node.js 官方提供的，例如 fs、path、http 等）
>
> 2、自定义模块（用户创建的每个 .js 文件，都是自定义模块）
>
> 3、第三方模块（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载）

### 加载模块

require() 方法，可以加载需要的内置模块、用户自定义模块、第三方模块进行使用

注意：

> 1、加载内置模块和第三方模块时，只需写模块名字；但是加载自定义模块，需要写路径（如：`require('./xxx.js')`）
>
> 2、加载自定义模块时，可以不写后缀名 如 .js .html 
>
> 3、使用 require() 方法加载其它模块时，会执行被加载模块中的代码
> 即：当模块内有 打印 等代码时，在require模块时就会执行 打印 等操作

#### require和import的不同

相同：引入复杂数据类型时，数据浅拷贝该对象

> 1、遵循规范
>
> - `require` 是 AMD规范引入方式
> - `import`是es6的一个语法标准，如果要兼容浏览器的话必须转化成es5的语法
>
> 2、调用时间
>
> - `require`是运行时调用，所以require理论上可以运用在代码的任何地方（虽然这么说但是还是一般放开头）
> - `import`是编译时调用，所以必须放在文件开头
>
> 3、本质
>
> - `require`是赋值过程，将需要导出的内容赋值给变量，且最终导出的内容永远是一个对象
> - `import`是解构过程（但是目前所有的引擎都还没有实现import，我们在node中使用babel支持ES6），最终导出的内容是定义的内容，定义number导出就是number,定义字符串就是字符串

### 模块作用域

> 定义：和函数作用域类似，在自定义模块中定义的变量、方法等成员，只能在当前模块内被访问，这种模块级别的访问限制，叫做模块作用域。
>
> 作用：防止全局变量污染的问题（浏览器中引入script，就没有模块作用域，会导致变量污染）

因此在nodeJS中如果需要调用其他模块的变量（/成员），需要将其共享（/暴露）出去

### 向外共享模块作用域中的成员

#### module

在每个 .js 自定义模块中都有一个 module 对象，它里面存储了和当前模块有关的信息

因为module对象是不需要创建的

![node_模块化_module](.\image\node_模块化_module.png)

常见属性：

> path：当前js文件所处的文件夹路径
>
> filename：当前js文件的绝对路径
>
> exports：其属性值是一个对象
>
> ​				其作用：**向外共享成员**

##### module.exports

在自定义模块中，可以使用 module.exports 对象，将模块内的成员共享出去，供外界使用。

外界用 require() 方法导入自定义模块时，得到的就是 module.exports 所指向的对象。

在自定义模块中，this默认指向module对象

如：

```js
//自定义模块
module.exports.myname = 'wz';
module.exports.SayHi = () => {
    console.log('你好哇' + this.myname);
}

//其他调用自定义模块
const x = require('./00-我是一个自定义模块');
console.log(x);				 //结果：{ myname: 'wz', SayHi: [Function (anonymous)] }
console.log(x.myname);		 //结果：'wz'
x.SayHi();					//结果：'你好哇wz'
```

##### exports

由于 module.exports 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了 exports 对象。**默认情况下，exports 和 module.exports 指向同一个对象**

注：最终共享的结果，还是以 **module.exports 指向的对象为准**

如：

```js
//自定义模块
module.exports.myname = 'wz';
module.exports.SayHi = () => {
    console.log('你好哇' + this.myname);
}
exports = {
    uname: 'w'
}

//其他调用自定义模块
const x = require('./00-我是一个自定义模块');
console.log(x);
//结果：{ myname: 'wz', SayHi: [Function (anonymous)] }
//并非exports的对象，而是module.exports的对象
```

又如：

```js
//自定义模块
exports.myname = 'wz';
exports.SayHi = () => {
    console.log('你好哇' + this.myname);
}
module.exports = {
    uname: 'w'
}

//其他调用自定义模块
const x = require('./00-我是一个自定义模块');
console.log(x);
//结果：{ uname: 'w' }
//此时module.exports新建的对象会覆盖之前的exports对象
```

再如：

```js
//自定义模块
exports = {
    uname: 'w'
}
module.exports.myname = 'wz';
module.exports.SayHi = () => {
    console.log('你好哇' + this.myname);
}

//其他调用自定义模块
const x = require('./00-我是一个自定义模块');
console.log(x);
//结果：{ myname: 'wz', SayHi: [Function (anonymous)] }
//此时exports指向了新对象，而非module.exports指向的对象
```

**结论**：**使用exports时，只能直接修改其成员，不能让exports指向一个新对象**
		如果要exports指向一个新对象，则需要写 `module.exports = exports`

因此，尽量避免同时使用module.exports和exports

### 模块化规范

Node.js 遵循了 CommonJS 模块化规范，CommonJS 规定了模块的特性和各模块之间如何相互依赖

CommonJS 规定：

> 1、每个模块内部，module 变量代表当前模块。
>
> 2、module 变量是一个对象，它的 exports 属性（即 module.exports）是对外的接口。
>
> 3、加载某个模块，其实是加载该模块的 module.exports 属性。require() 方法用于加载模块

------

## npm & 包

### 包

> 定义：Node.js 中的第三方模块又叫做包。包是基于内置模块封装出来的，提供了更高级、更方便的 API，极大的提高了开发效率
>
> 需要包的原因：由于 Node.js 的内置模块仅提供了一些底层的 API，导致在基于内置模块进行项目开发的时，效率很低
>
> 下载包的途径：国外有一家 IT 公司，叫做 npm, Inc，它是全球最大的包共享平台。
> 该公司提供了一个地址为 https://registry.npmjs.org/ 的服务器，来对外共享所有的包，我们可以从这个服务器上下载自己所需要的包。
>
> > 从 https://www.npmjs.com/ 网站上搜索自己所需要的包
> >
> > 从 https://registry.npmjs.org/ 服务器上下载自己需要的包
>
> 下载包的方法：
>
> > npm, Inc. 公司提供了一个包管理工具，我们可以使用这个包管理工具，从 https://registry.npmjs.org/ 服务器把需要的包下载到本地使用。
> >
> > 这个包管理工具的名字叫做 Node Package Manager（简称 npm 包管理工具），这个包管理工具随着 Node.js 的安装包一起被安装到了用户的电脑上。

### 安装包的方法

> 1、在项目文件中，打开终端
>
> 2、输入`npm install 包的名称  `或  `npm i 包的名称`
>
> 3、如果要安装具体版本的包，只需在 包的名称 后面加 @2.2.2 如：`npm i 包的名称@version`

安装后，目录下会多两个文件：node_modules文件夹 和 package-lock.json

> 1、node_modules：用来存放所有已安装到项目中的包。require() 导入第三方包时，就是从这个目录中查找并加载包
>
> 2、package-lock.json：用来记录 node_modules 目录下的每一个包的下载信息，例如包的名字、版本号、下载地址等

### 包管理配置文件

npm 规定，在项目根目录中，必须提供一个叫做 **package.json** 的包管理配置文件。用来记录与项目有关的一些配置信息

分享代码的时候，不需要分享包（包太大了），其他人使用分享的代码时，只需重新去下载包即可使用

#### dependencies

> package.json文件中，dependencies里面记录了安装的包的名称和版本
>
> dependencies保存的包会在开发和项目上线之后都需要用到

因此，**其他人使用分享的代码时，直接cmd：`npm install`，就会自动下载package.json文件中记录的全部包**

#### devDependencies

> 如果某些包只在项目开发阶段会用到，在项目上线之后不会用到，则建议把这些包记录到 devDependencies 节点中

将指定的包安装在devDependencies的方法：

`npm i 包的名称 -D`			或		`npm install 包的名称 --save-dev`		或		`npm install --save-dev 包的名称 `

### 卸载包

`npm uninstall 包的名称  `

会卸载该包，且会将该包名称从package.json文件中的dependencies删除

### 淘宝 NPM 镜像服务器

**镜像**（Mirroring）是一种文件存储形式，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像。

安装cnpm：`npm install -g cnpm --registry=https://registry.npm.taobao.org`

### 包的分类

> 1、项目包
>
> 2、全局包

#### 项目包

> 定义：被安装到项目的 node_modules 目录中的包，都是项目包
>
> 分类：
>
> > 1、开发依赖包（被记录到 devDependencies 节点中的包，只在开发期间会用到）
> >
> > 2、核心依赖包（被记录到 dependencies 节点中的包，在开发期间和项目上线之后都会用到）

#### 全局包

> 定义：在执行 npm install 命令时，如果提供了 -g 参数，则会把包安装为全局包。即：`npm i 包的名称 -g`
>
> 卸载：`npm unistall 包的名称 -g`
>
> 安装位置：全局包会被安装到 C:\Users\用户目录\AppData\Roaming\npm\node_modules 目录下
>
> 注：只有工具性质的包，才有全局安装的必要性。因为它们提供了好用的终端命令

### 开发自己的包

步骤：

> 1、初始化包的基本结构
>
> > 新建 xxx 文件夹（名字可以自己取），作为包的根目录
> >
> > 在 xxx 文件夹中，新建如下三个文件：
> >
> > > lpackage.json （包管理配置文件）
> > >
> > > lindex.js     （包的入口文件）
> > >
> > > README.md （包的说明文档）
>
> 2、初始化 package.json
>
> ```json
> {
>     "name": "xxx",
>     "version": "1.0.0",
>     "main": "index.js",
>     "description": "描述该包的功能",
>     "keywords": ["x","y"],
>     "license": "ISC"
> }
> ```
>
> 属性名：
>
> > name：是指该包的名字（与xxx 文件夹的 xxx同名）	包名不能与npm官网的重复
> >
> > version：版本号（点分十进制，第一个数字代表大版本，第二个数字代表功能更新的版本，第三个数字代表bug更新）
> >
> > main：包的入口文件（外界require导入包时，就是导入main属性下的入口文件）
> >
> > description：包的简短描述信息
> >
> > keywords：在npm官网搜索的关键字，其值是数组，数组内必须是字符串
> >
> > license：包所遵循的开源许可协议，npm默认是ISC
>
> 3、开发包所需实现的功能
>
> 4、将不同的功能进行模块化拆分
>
> > 相当于嵌套，即index.js文件里面不写 实现功能 的代码，而是直接require引用子文件
> >
> > 子文件：实现指定功能，并暴露module.exports = { Fn }		Fn为函数名
> > 注：子文件一般放在src文件夹下
> >
> > ndex.js文件：require子文件，再暴露 module.exports = { Fn1 , Fn2 }		Fn1来自子文件1，Fn2来自子文件2
> >
> > 如：
> >
> > ```js
> > //子文件1 (son1.js)
> > let Fn1 = () => {}
> > module.exports = { Fn1 }
> > 
> > //子文件2 (son2.js)
> > let Fn2 = () => {}
> > let Fn3 = () => {}
> > module.exports = { Fn2 , Fn3 }
> > 
> > //父文件 (index.js)
> > const son1 = require('./src/son1.js');
> > const son2 = require('./src/son2.js');
> > module.exports = {
> >     ...son1,
> >     ...son2
> >     //使用扩展运算符：这样就可以将子文件的exports对象展开，就省去了繁琐的代码
> >     //...son2 相当于：son2.Fn2 , son2.Fn3
> > }
> > ```
>
> 5、包根目录中的 README.md 文件，是包的使用说明文档。通过它，我们可以事先把包的使用说明，以 markdown 的格式写出来，方便用户参考。（相当于一个使用说明书）
>
> > README.md一般包括：安装方式、导入方式、包里面的函数名及其功能
>
> 6、发布包
>
> > (1)  在终端中执行 npm login 命令，依次输入用户名、密码、邮箱后，即可登录成功
> >
> > (2)  将终端切换到包的根目录之后，运行 npm publish 命令，即可将包发布到 npm 上
> >
> > (3)  运行 npm unpublish 包名 --force 命令，即可从 npm 删除已发布的包。（上传72h后就不能删了）

### 模块的加载机制

> 优先从缓存中加载：模块在第一次加载后会被缓存。 这也意味着**多次调用 require() 不会导致模块的代码被执行多次**
>
> 内置模块的加载机制：内置模块是由 Node.js 官方提供的模块，内置模块的加载优先级最高
>
> 自定义模块的加载机制：使用 require() 加载自定义模块时，必须指定以 ./ 或 ../ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 ../ 这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。
> 导入自定义模块时，可以省略了文件的扩展名，但其只能匹配js、json、node文件，其他文件没法自动补全
>
> 第三方模块的加载机制：从 /node_modules 文件夹中加载第三方模块

------

## express

> 定义：Express 是基于 Node.js 平台，快速、开放、极简的 Web 开发框架
>
> 作用：Express 的作用和 Node.js 内置的 http 模块类似，是专门用来创建 Web 服务器的（用法比http更方便，功能更强大）
>
> 本质：就是一个 npm 上的第三方包（基于内置的 http 模块进一步封装出来的），提供了快速创建 Web 服务器的便捷方法

### 初识express

#### 创建基本的 Web 服务器

> 1、导入express包
>
> ```js
> const express = require('express');
> ```
>
> 2、创建web服务器
>
> ```js
> const app = express();
> ```
>
> 3、调用listen方法，启动服务器
>
> ```js
> app.listen(num, callback)
> ```
>
> 参数：
>
> > num：端口号，数字型
> >
> > callback：回调函数，用户访问服务器后服务器做出的响应操作

完整代码：

```js
//1.导入express包
const express = require('express');
//2.创建web服务器
const app = express();
//3.调用listen方法，启动服务器
app.listen(80, () => {
    console.log('express server running at http://127.0.0.1:80');
})
```

#### 监听GET请求

> 语法：`app.get('url', function(req , res))`
>
> 参数：
>
> > url：客户端请求的 url 地址
> >
> > req：请求对象（request）
> >
> > res：响应对象（response）

代码：

```js
app.get('/', (req, res) => {});
app.get('/user', (req, res) => {});
```

#### 监听POST请求

> 语法：`app.post('url', function(req , res))`
>
> 参数：
>
> > url：客户端请求的 url 地址
> >
> > req：请求对象（request）
> >
> > res：响应对象（response）

代码：

```js
app.post('/', (req, res) => {});
app.post('/user', (req, res) => {});
```

##### req

常用属性：

> req.query：可以访问到客户端通过查询字符串的形式，发送到服务器的参数
>
> > 语法：`req.query`
> >
> > 如：
> >
> > ```js
> > 客户端使用 ?name=wz&age=18
> > 这就是查询字符串的形式
> > //服务器可以用req.query.name等查询到值
> > 
> > app.get('/user', (req, res) =>{
> >     console.log(req.query);
> >     console.log(req.query.name);
> > })
> > //当客户端输入 http:127.0.0.1?name=wz&age=18
> > //此时服务器会打印：{ name: 'wz', age: 18 } 和 'wz'
> > ```
>
> req.params：可以访问到 URL 中，通过  **:**  匹配到的动态参数（默认为空对象）
>
> > 语法：`req.params`
> >
> > 如：
> >
> > ```js
> > app.get('/user/:id', (req, res) =>{
> >     console.log(req.params);
> > })
> > 
> > //当客户端输入 http:127.0.0.1/user/5
> > //此时服务器会打印：{ "id": "5" }
> > ```
> >
> > 又如：
> >
> > ```js
> > app.get('/user/:id/:name', (req, res) =>{
> >     console.log(req.params);
> > })
> > 
> > //当客户端输入 http:127.0.0.1/user/1/wz
> > //此时服务器会打印：{ "id": "1", "name": "wz" }
> > ```
> >
> > 注： **:** 后面的字符串 是指参数的名字，参数的值是动态匹配的
>
> req.body：接受客户端发送过来的请求体数据
>
> > 语法：`req.body`
> >
> > 注：不配置解析表单数据的中间件（即不设置`express.json()`或`express.urlencoded()`），则默认为undefined

##### res

常用方法：

> res.send()：向客户端发送括号内的内容（内容格式在客户端那边会隐式转换为JSON格式）

#### app.use()

结合后面的 express中间件来学习

> 作用：注册全局中间件		常用于托管静态资源、路由挂载等
>
> 语法：`app.use()`

#### 解决跨域

cors方法

```js
//本地服务器解决跨域
app.all('*', function (req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');	//这一句就是解决跨域
    //Access-Control-Allow-Headers ,可根据浏览器的F12查看,把对应的粘贴在这里就行
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    res.header('Access-Control-Allow-Methods', '*');		//允许请求方法：如post、get、put
    res.header('Content-Type', 'application/json;charset=utf-8');
    next();
});
```

#### 托管静态资源

> 作用：通过它，我们可以非常方便地创建一个静态资源服务器，外部就可以通过url输入访问到指定文件夹下的所有资源
>
> 语法：`app.use(express.static('文件夹'))`
> 此时就托管了‘文件夹’下的所有文件（包括子文件夹的内容）
>
> 参数：文件夹：做静态资源的文件夹，托管后，该文件夹下的所有文件就可以通过url输入的方式访问
>
> 如：`http://127.0.0.1:80/JS/index.js`
>
> 注：
>
> > 1、**文件夹的名称不需要在url地址中出现**
> >
> > 2、如果需要托管多个静态资源目录，请多次调用 express.static() 函数
> >
> > 如果多个静态资源文件夹都有同名文件，则会按照顺序查找（先找第一个文件夹，如果没有就找第二个；如果有就不往后面找了）
> >
> > 3、路径是相对于该js文件的，但有时候相对路径无效，可以用__dirname来拼凑绝对路径

挂载路径前缀

> 因为正常情况下 文件夹的名称不需要在url地址中出现
>
> 如果需要在url中出现文件夹的名称，则需要挂载路径前缀
>
> 语法：`app.use('/文件夹', express.static('文件夹'))`
>
> 如：`http://127.0.0.1:80/clock/JS/index.js`
>
> 注：访问前缀可以是任意值，不一定非要是文件夹的名称

### express 路由

路由：

> 定义：广义上来讲，路由就是映射关系；

express 路由

> 定义：express中是指的是客户端的请求与服务器处理函数之间的映射关系
>
> 3 部分组成，分别是请求的类型、请求的 URL 地址、处理函数：
>
> > 语法：`app.METHOD(PATH, HANDLER)`
> >
> > 如：`app.post('/user', (req,res) => {})`

路由的匹配过程

> 在匹配时，会按照路由的顺序进行匹配，如果请求类型和请求的 URL 同时匹配成功，则 Express 会将这次请求，转交给对应的 function 函数进行处理。
>
> ![node_express_路由](.\image\node_express_路由.png)
>
> 注：
>
> > 1、按照定义的先后顺序进行匹配
> >
> > 2、请求类型和请求的URL同时匹配成功，才会调用对应的处理函数

#### express路由的使用

##### 直接挂在到app上

如：`app.post('/user', (req,res) => {})`

问题：随着代码量增多，挂载的路由会增加，文件体积过大+

##### 模块化路由

作用：为了方便对路由进行模块化的管理，express 不建议将路由直接挂载到 app 上，而是推荐将路由抽离为单独的模块。

步骤：

> 1、创建路由模块对应的 .js 文件
>
> 2、调用 express.Router() 函数创建路由对象
>
> 3、向路由对象上挂载具体的路由
>
> 4、使用 module.exports 向外共享路由对象
>
> 5、使用 app.use() 函数注册路由模块

路由模块的代码：

```js
//1.导入express
const express = require('express');
//2.创建路由对象
const router = express.Router();
//3.挂载用户的路由
router.get('/user', (req, res) => {
    res.send('欢迎使用GET请求');
});
router.post('/', (req, res) => {
    res.send('欢迎使用POST请求');
});
//4.向外暴露路由
module.exports = router;
```

服务器调用路由模块的代码：

```js
const express = require('express');
const app = express();

//1.导入路由模块 
const router = require('./3-路由模块');

//2.使用app.use()注册路由模块
app.use(router);

app.listen(8080, () => {
    console.log('express server is running at http://127.0.0.1:8080');
})
```

为路由模块添加前缀：

同托管静态资源的挂在路径前缀一样

如：`app.use('/abab', router);`

### express中间件

> 作用：
>
> > 1、当一个请求到达 express 的服务器之后，可以连续调用多个中间件，从而对这次请求进行**预处理**
> >
> > 2、多个中间件之间，**共享同一份** **req** **和** **res**。基于这样的特性，我们可以在上游的中间件中，**统一**为 req 或 res 对象添加自定义的属性或方法，供下游的中间件或路由进行使用。
> > 因此，要使中间件的变量能在后面的中间件或路由中使用时，必须将其追加到res或req对象上
>
> 流程：上一个中间件的输出会作为下一个中间件的输入
>
> ![node_express_中间件](.\image\node_express_中间件.png)
>
> 本质：就是一个function处理函数
>
> 注意事项：
>
> > 1、一定要在路由之前注册中间件（否则中间件不起作用）
> >
> > 2、客户端发送过来的请求，可以连续调用多个中间件进行处理
> >
> > 3、执行完中间件的业务代码之后，不要忘记调用 next() 函数
> >
> > 4、为了防止代码逻辑混乱，调用 next() 函数后不要再写额外的代码
> >
> > 5、连续调用多个中间件时，多个中间件之间，共享 req 和 res 对象

#### 中间件处理函数

> 定义：服务器会先执行中间件，然后该中间件的next()指向下一个中间件，如果没有下一个中间件，则指向路由(`app.get()`)
>
> 语法：
>
> ```js
> function (req, res, next){
> 	next();
> }
> ```
>
> 注：中间件形参必须包含next参数，且中间件的业务处理完毕后必须调用next函数
>
> 如：
>
> ```js
> let mv = (req, res, next) => {
>     doSomethings;
>     next();
> }
> ```
>
> next函数的作用：next 函数是实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件或路由
> next不需要人为书写

#### 全局生效中间件

> 定义：客户端发起的任何请求，到达服务器之后，都会触发的中间件，叫做全局生效的中间件
>
> 创建步骤：
>
> > 1、创建一个中间件
> >
> > 2、利用app.use(mv)，将mv这个中间件全局生效
>
> 定义多个全局中间件：使用 app.use() 连续定义多个全局中间件，服务器会按照中间件定义的先后顺序依次进行调用，

```js
let mv = (req, res ,next) => {
    doSomethings;
    next();
}
app.use(mv);
```

#### 局部生效中间件

> 定义：不使用 app.use() 定义的中间件，叫做局部生效的中间件
>
> 使用方法：将中间件处理函数放在 `app.get('/', mv, (req, res) => {})`
> 或： `app.get('/', mv1, mv2, (req, res) => {})`
> 或： `app.get('/', [mv1, mv2], (req, res) => {})`
>
> 如：
>
> ```js
> const express = require('express');
> const app = express();
> 
> let mv1 = (req, res, next) => {
>    	let one = '我是局部生效中间件mv1';
>    	req.one = one;
>    	next();
> }
> 
> let mv2 = (req, res, next) => {
>    	let two = '我是局部生效中间件mv2';
>    	req.two = two;
>    	next();
> }
> 
> app.get('/', mv1, (req, res) => {
>    	res.header({ 'Access-Control-Allow-Origin': '*' });
>    	res.send(req.one);
> });
> 
> app.get('/user', mv2, (req, res) => {
>    	res.header({ 'Access-Control-Allow-Origin': '*' });
>    	res.send(req.two);
> });
> 
> app.listen(8080, () => {
>    	console.log('express server is running at http://127.0.0.1:8080');
> })
> ```

#### 中间件的分类

> 1、应用级别的中间件：通过 app.use() 或 app.get() 或 app.post() ，绑定到 app 实例上的中间件，叫做应用级别的中间件
>
> 2、路由级别的中间件：绑定到 express.Router() 实例上的中间件，叫做路由级别的中间件。它的用法和应用级别中间件没有任何区别。只不过，应用级别中间件是绑定到 app 实例上，路由级别中间件绑定到 router 实例上
>
> 3、错误级别的中间件
>
> 4、express 内置的中间件
>
> 5、第三方的中间件

##### 错误级别的中间件

> 作用：专门用来捕获整个项目中发生的异常错误，从而防止项目异常崩溃的问题
> 正常访问，如果出现错误，可能项目无法正常执行，且不知道错误原因；但是有了错误级别的中间件，可以保证除该模块外的代码能够执行
>
> 语法：`function (err, req, res, next) {}`
>
> 参数：
>
> > err：错误信息，是一个对象，其常用属性err.mesage
>
> 注：**错误级别的中间件，必须注册在所有路由之后**
>
> 如：
>
> ```js
> app.get('/', (req, res) => {
>     throw new Error('服务器内部发生了错误！');	//自定义一个错误，在执行此次语句时，将不再执行当前块级下的后面的语句
>     res.send('xxx');
> });
> app.use((err, req, res, next) =>{		//错误级别的中间件，在get错误时，会执行该代码
>     res.send('Error!' + err.message);	//此时的err.message就是throw new Error括号内的信息
> })
> ```

##### express内置的中间件

> 1、express.static()： 快速托管静态资源的内置中间件，例如： HTML 文件、图片、CSS 样式等
>
> 2、express.json()： 解析 JSON 格式的请求体数据
>
> > 语法：`app.use(express.json())`
> >
> > 括号内不需要写任何东西，因为其解析完毕后，这个中间件会自动将解析后的数据放在req里面
>
> 3、express.urlencoded()： 解析 URL-encoded 格式的请求体数据
>
> > URL-encoded 格式：`application/x-www-form-urlencoded`
> > 即：键值对的格式
> >
> > 语法：`app.use(express.urlencoded({ extended: false }))`
>
> 注：上述都是放在app.use中

##### 第三方的中间件

> 定义：非 Express 官方内置的，而是由第三方开发出来的中间件，叫做第三方中间件。
>
> 如：在 express@4.16.0 之前的版本中，经常使用 body-parser 这个第三方中间件，来解析请求体数据。使用步骤如下：
>
> > ① 运行 npm install body-parser 安装中间件
> >
> > ② 使用 require 导入中间件
> >
> > ③ 调用 app.use() 注册并使用中间件
> >
> > 代码：
> >
> > ```js
> > const parser = require('body-parser');
> > app.use(parser.urlencoded({ extended: false }));
> > ```
>
> 注：express 内置的 express.urlencoded 中间件，就是基于 body-parser 这个第三方中间件进一步封装出来的
