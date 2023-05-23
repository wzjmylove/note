# 锚点

[webpack加载全部模块的原因](##模块化原理)

# webpack

## 模块化原理

webpack支持多模块化

编译原理：AST抽象语法分析的工具，分析出代码中js文件有哪些导入和导出操作；
但又因为需要将不同的导入导出统一成其自己的逻辑，因此需要将所有的依赖全部读一遍，因此项目越大、模块越多，启动开发服务器的时间就越长

如：

```js
const lodash = require('lodash');	 // commonjs规范
import Vue from 'vue';				// es6 module规范
```

webpack转换后

```js
const lodash = webpack_require('lodash');
const Vue = webpack_require('vue');
```

