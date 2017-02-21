## 路由的设计

### 认识路由对象

- path
- params
- query
- app

> /path/submodule/:params?query



```js

# /student/score/:name

!#/student/score/:jack?period=201704&subject=english

{ 
  path: '/student/score',
  params: {name: 'jack'},
  query: { period: '201704', subject: 'english'}
}
```



- 建议1：path/subPath/能够对应功能模块
    - 原因：文件的目录结构清晰，路由规则建档明确


```sh
  
├── datacenter
├── management
└── tools
    ├── AdPreview
    ├── AppList
    ├── BatchTask
    ├── History
```

** 二级路由结构

```json
[
    { path: 'datacenter', component: DataCenter },
    { path: 'management', component: Management },
    {
        path: 'tools',
        component: Tools,
        children: [
            { path: 'AdPreview', component: AdPreview },
            { path: 'AppList', component: AdPreview },
            { path: 'BatchTask', component: BatchTask },
            { path: 'History', component: History }
        ]
    }
]
```

- 建议2：如果项目导航结构是多层级，建议使用子路由规则
    - 如果模块少，不存在多级导航，可以用一级别路由
    - 多层级目录；多层级导航；动态加载情况下使用子路由
    
- 建议3：params慎用，除非是为了动态路由/切换状态
    - params容易与子路由冲突
    - params扩展不方便，而且容易造成path过长
    - params不适合参数更新频繁情况
    - params不如query参数语义不明确

- 建议4: query与store建立映射关系
    
    - query是key/value状态信息
    - 通过全局store组件能够方便获取当前的状态

- 建议5：切换组件的状态保持active

    - 组件的mounted 之前的生命周期不会触发
    - 切换不同子组件可能引入数据污染（data/class/attr）



```html
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```


    
- 建议6：动态配置路由的情况

    - 需要请求后端修改或添加路由表（芳华创意）
    - 大部分情况是追加规则，但是规则可能冲突或重复
    - 可以设置权限（条件）设置
    - 可以使用动态模块解析
    - 可以watch($router)

- 建议7：懒加载


```js
const Foo = resolve => {
  // require.ensure 是 Webpack 的特殊语法，用来设置 code-split point
  // （代码分块）
  require.ensure(['./Foo.vue'], () => {
    resolve(require('./Foo.vue'))
  })
}
```

- 建议8：关注变化

- 监听router对象的befereChange/afterChange钩子
- 关注组件的beforeRouteEnter/beforeRouteLeave、合理预备与善后
- 可以watch组件的$route参数变化，响应内容组件


### 其它建议

- 设置404页面
- 设置跳转页面
- router
- 命名视图：有时候想同时（同级）展示多个视图
- 视图切换：视图状态保存


## 权限控制

> 通过路由钩子函数和元信息设置权限控制


```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```