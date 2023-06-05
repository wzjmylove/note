# v4和v2的区别

1、挂载方式不一样

2、路由模式的写法不一样，v2是mode+history/hash，v4则是history+createWebHistory()/createHashHistory

# 配置

在项目根目录下创建router文件夹，里面再创一个index文件

```ts
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router'
import Home from '../views/Home.vue'

const routes = <RouteRecordRaw[]>[
  {
    path: '/home',
    component: Home,
  },
  {
    path: '/about',
    component: () => import('../views/About.vue'),  // 懒加载，只有当url跳转到该路由时才渲染该页面，以优化性能
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router;
```

main.ts里面进行注册

```ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')
```

# 使用

## 编程式跳转

### 命名路由

上述配置里面只写了url，可以通过`<router-link to="/"></router-link>`或`router.push('/')`或`router.push({ path: '/' })`实现跳转

如果对其进行了命名，即：

```ts
{
    path: '/home',
    name: 'HOME',
    component: Home,
},
```

则可以通过`<router-link :to="{ name="HOME" }"></router-link>`或`router.push({ name: 'HOME' })`实现跳转

### 历史记录

#### 有历史记录

默认的router-link和router.push()都会留下历史记录

在有历史记录的情况下，可以通过`router.go(num)`和`router.back()`来实现对历史记录的访问

> 语法：`router.go(num)`
>
> 参数：num，必填，数字型，表示页面前进几次，可以为负数，比如2就表示前进2次

> 语法：`router.back()`
>
> 作用：表示页面后退一次

#### 无历史记录

可以通过replace来去掉历史记录

> 用法：`<router-link replace to="" />`或`router.replace()`

### 