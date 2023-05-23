# 锚点

[提高项目性能](###导出)

# Vite

vite是vue团队的官方出品，支持构建vue、react、angular、svelte项目

概念：思维比较前卫而且先进的构建工具（目前只基于浏览器项目），解决了一些webpack解决不了的问题，同时降低了一些心智负担

起因：项目越来越大时，构建工具（webpack）所要处理的js代码就越多，需要很长时间才能启动开发服务器（webpack没法更改，因为关系到其底层逻辑）

生产环境时，vite会全权交给rollup的库去完成生产环境的打包

## 相较于webpack的优劣势

### 优势

> 1、按需加载模块，webpack则是加载全部模块（[原因](.\02-Webpack.md)）
>
> 2、启动时间短

### 劣势

但是vite不会把webpack完全替代：因为项目有可能不只是运行在浏览器端，也有可能是服务器端，此时就需要构建工具支持require导入等node的语法，但是**vite只允许es module语法**的导入

即：

> webpack更多的关注兼容性
>
> vite关注浏览器端的开发体验

## Vite脚手架

### 使用

和webpack都是开箱即用（out of box）

使用场景：开发环境

#### 手动使用

```shell
yarn add vite -D
# or
npm i vite -D
```

然后在package.json里面添加

```json
"scripts": {
	"dev": "vite",	// 代表开发环境
    "build": "vite build"	// 代表生产环境
}
```

此时就手动搭建了一个vite脚手架，然后就可以直接 `yarn dev` 启动项目了

#### 自动使用

利用create-vite实现自动创建vite项目

```shell
npm create vite 项目名
# 或者
yarn create vite 项目名
```

此时执行的操作是：

> 1、全局安装create-vite（vite的脚手架），以帮助开发者搭建预设（如搭建main.js、src文件夹等）
>
> 2、直接运行create-vite的bin目录下的一个执行配置

注：搭建预设是create-vite做的，而非vite做的

### create-vite和vite的关系

create-vite内置了vite

就好比用vue-cli会内置webpack一样

## Vite的预加载

会在启动项目时，本地执行预加载

### 导入

注意vite只支持import导入

在处理代码的过程中如果遇到了非绝对路径或相对路径的引用，就会开启路径补全
查找过程：当前目录开始依次向上查找，直到找到或到根目录为止

如：

```js
import lodash from "lodash"
// vite补全路径后
import lodash from "/node_modules/.vite/deps/lodash.js"
```

### 导出

有的插件可能不是esModule导出（export导出），而是commonJS导出（module.export导出）

利用依赖预构建：

vite先找到对应依赖，然后调用esbuild，将其他规范的代码转换成esModule规范，然后放到当前目录下的node_module/.vite/deps；同时对esModule规范的各个模块进行了统一集成

如：

```js
// 对esModule规范的各个模块进行了统一集成
// 一个插件可能引入其他依赖，vite对其进行处理后，会将其引入的其他依赖（即当前插件的import）全部改写成函数，避免了依赖的套用，浏览器的网络上就会只加载一个依赖文件

import fn from "fn"
// 改写后
function fn () {	// 该函数与fn依赖里的fn函数功能一样
    .....
}
```

预加载前：![vite_预加载前](.\image\vite_预加载前.png)

预加载后：![vite_预加载后](.\image\vite_预加载后.png)

因此预加载能够使vite尽可能的将依赖套用集成为一个或几个依赖

依赖预构建解决的问题/优势：

> 1、解决不同的导出格式
>
> 2、对路径的处理上可以直接使用.vite/deps，方便路径重写
>
> 3、网络多包传输性能优化（因为一个插件可能会引入其他的插件，因此原生esModule必须使用路径引入，其的[解决方法](###导出)）

## 配置项vite.config.js

vite.config.js也可以module.export导出，因为vite会先调用esbuild将其转换为esModule规范

### 语法提示

vite.config.js可以支持直接导出一个对象，也可以导出一个defineConfig，更推荐第二个，因为会有语法提示

```js
import { defineConfig } from "vite"
export default defineConfig({})		// 本质就是defineConfig函数return的对象有ts的类型注释


// 用注释 + ts类型 也能得到语法提示


/** @type import('vite').UserConfig  */
const viteConfig = {}
export default viteConfig
```

### 环境处理

环境控制用defineConfig

```js
import { defineConfig } from "vite"
export default defineConfig(({ command }) => {
    if(commond === "build")
        // 代表开发环境的配置
    if(commond === "serve")
        // 代表生产环境的配置
})
```

环境变量：会根据当前的代码环境变化而变化的变量叫做环境变量

代码环境：开发环境、测试环境、预发布环境、灰度环境、生产环境

#### env文件

对于环境变量，是需要将其放置于.env文件中，此时vite内置的dotenv这个三方库会自动读取该文件，并将其添加到[process对象](..\JS笔记\06-nodeJS.md)下













```js
optimizeDeps: {
	exclude: ["xxx"]	// 将指定数组中的依赖不进行预构建
}
```

