---
doc: true
title: vue-router
date: 2018-08-29 10:08:09
tags: vue-router
---
学习 [vue-router](https://router.vuejs.org/zh/guide/#html) 的笔记

<!--more-->

## 安装

```
npm install vue-router
```

在一个模块化工程中使用，必须要通过 Vue.use() 明确地安装路由功能：

```
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(vueRouter)
```

## 起步

将组件（components）映射到路由（routes），然后告诉 Vue Router 在哪里渲染

html:
```
<div id="app">
    <p>
        <!--使用 router-link 组件来导航-->
        <!--通过传入 to 属性指定链接-->
        <!--router-link 会被渲染成一个 <a> 标签-->

        <router-link to="/foo">Go to Foo</router-link>
        <router-link to="/bar">Go to bar</router-link>
    </p>

    <!--路由出口-->
    <!--路由匹配到的组件将在这里渲染-->

    <router-view></router-view>
</div>
```

js:
```
// 0. 若使用模块化编程，导入 Vue 和 VueRouter , 调用 Vue.use(VueRouter)

// 1. 定义组件。 也可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>'}
const bar = { template: '<div>bar</div>'}

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
const routes = [
    {path: '/foo', component: Foo},
    {path: '/bar', component: Bar}
]

// 3. 创建 router 实例
const router = new VueRouter({
    routes: routes    // 可以直接写 routes
})

// 4. 创建和挂载根实例
// 要通过 router 配置参数注入路由，从而让整个应用都有路由功能
// Vue 的$mount()为手动挂载。new Vue时，el和$mount并没有本质上的不同。
const app = new Vue({
    router
}).$mount('#app')
```

通过注入路由器，我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由

## vue 路由传参

两种方式：params、query。接受参数都是 this.$route.params.name 和 this.$route.query.name。
注意接受参数时，已经是 $route 了，而不是 $router 。

params 只能用 name 来引入路由
```
this.$router.push({
    name: 'detail',
    params:{
        name: 'nameValue',
        code: 1001
    }
});

// url 表现形式 不带参数
```

query 要用 path 来引入
```
this.$router.push({
    path: 'detail',
    query： {
        name: 'nameValue',
        code: 1001
    }
})

// url 表现形式 带参数
```

## 动态路由匹配

可以使用 动态路径参数

```
const User = {
    template: '<div>User</div>'
}

const router = new VueRouter({
    routes: [
        // 动态路径参数 以冒号开头
        {path: '/user/:id', component: User}
    ]
})
```

响应路由变化的参数

```
conse User = {
    template: '...',
    watch: {
        '$route'(to, from) {
            //  对路由变化作出响应...
        }
    }
}

或者使用 beforeRouteUpdate 导航守卫

conse User = {
    template: '...',
    watch: {
       beforeRouteUpdate (to, from, next) {
            // react to route changes...
            // don't forget to call next()
       }
    }
}

```

## 嵌套路由

一个被渲染组件同样可以包含自己的嵌套 <router-view> 。

```
const User = {
    template: '
        <div>
            <h2>User {{$route.params.id}}</h2>
            <router-view></router-view>
        </div>
    '
}
```

要在嵌套的出口中渲染组件，需要在 VueRouter 的参数中使用 children 配置：

```
const router = new VueRouter({
    routes: [
       {
            path: '/user/:id',
            component: User,
            children: [
                {
                    // 当 /user/:id/profile 匹配成功，
                    // UserProfile 会被渲染在 User 的 <router-view> 中
                    path: 'profile',
                    component: UserProfie
                },
               {
                    // 当 /user/:id/posts 匹配成功，
                    // UserProfile 会被渲染在 User 的 <router-view> 中
                    path: 'posts',
                    component: UserProsts
               }，
               {
                    // 空的子路由
                     path: '',
                     component: UserHome
               }
            ],
       }
    ]
})
```

ps: 以 / 开头的嵌套路径会被当作根路径。这让你充分的使用嵌套组件而无须设置嵌套的路径。

## 编程式导航

```
router.push(location, onComplete?, onAbort?)

// ps: 在 Vue 实例内部，可以通过 $router 访问路由实例。因此可以调用 this.$router.push

```

```
// 声明式
<router-link :to="...">
// 编程式
router.push(...)
```

该方法的参数可以是一个字符串路径，或者描述地址的对象。

```
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({name: 'user',params: {userId:123}})

// 带参查询，/register?plan=private
router.push({ path: 'register', query: {plan: 'private'}})

// 手写完整的带参数path
router.push({ path: '/user/${userId}' })
```

其他方法
```
router.replace(location, onComplete?, onAbort?)  // 不会向 history 添加新记录
router.go(n)  // 参数为整数，在 history 中向前或者后退多少步
```

## 命名路由

```
const router = new VueRouter({
    routes: [
        {
            path: '/user/:userId',
            name: 'user',
            component: User
        }
    ]
})

<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

## 命名视图

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>

const router = new VueRouter({
    routes: [
        {
            path: '/',
            components: {
                default: Foo,
                a: Bar,
                b: Baz
            }
        }
    ]
})
```

## 重定向

当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b

```
const router = new VueRouter({
    routes: [
        { path: '/a', redirect: '/b'}
    ]
})

// 命名的路由
const router = new VueRouter({
    routes: [
        { path: '/a', redirect: { name: 'foo' }}
    ]
})

// 方法
const router = new VueRouter({
    routes: [
        { path: '/a', redirect: to => {
            // 方法接收 目标路由 作为参数
            // return 重定向的 字符串路径/路径对象
        }}
    ]
})
```

## 别名

/a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。

```
const router = new VueRouter({
    routes: [
        { path: '/a', component: A, alias: '/b'}
    ]
})
```

## 路由组件传参

通过 props 将组件和路由解耦

```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

## 路由元信息

定义路由的时候可以配置 meta 字段：

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})


一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 $route.matched 数组。
因此，我们需要遍历 $route.matched 来检查路由记录中的 meta 字段。
```
