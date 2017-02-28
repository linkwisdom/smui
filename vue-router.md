## 路由的设计

### 认识路由对象

- *配置*
  - path
  - params
  - query
  - hash
  - fullPath
  - matched
  - name

- *实例属性*

  - router.app
  - router.mode
  - router.currentRoute


- *实例方法*

  - router.beforeEach(guard)
  - router.afterEach(hook)
  - router.push(location)
  - router.replace(location)
  - router.go(n)
  - router.back()
  - router.forward()

### 路由格式

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




### 路由格式设计建议

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

** 二级路由结构 **

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

- 建议6：使用参数动态控制子视图模块
    - 需要请求后端修改或添加路由表（芳华创意）
    - 大部分情况是追加规则，但是规则可能冲突或重复
    - 可以设置权限（条件）设置
    - 可以使用动态模块解析
    - 可以watch($router)

```js
    // 路由参数变化驱动子视图变化
    
   router.query = {
       view: 'Report'
   }
   
   // 注册视图组件，通过路由参数控制子视图
   components: {
       Report,
       Editor,
       TableList
   }
```

```html
    <component is="view"></component>
```

- 建议7：懒加载


```js
const Foo = resolve => {
    // （代码分块）
    // webpack 使用require.ensure
    // webpack2.0 使用 system.import
    // fis/AMD模式使用 require(['./Foo.vue'], resolve)
    require.ensure(['./Foo.vue'], () => {
        resolve(require('./Foo.vue'))
    })
}
```

- 建议8：关注变化

    - 监听router对象的`befereEach`/`afterEach`钩子
    - 关注组件的`beforeRouteEnter`/`beforeRouteLeave`、合理预备与善后
    - 可以watch组件的`$route`参数变化，响应内容组件
    - 设置alias和replace
    
    
```
    routerConf = [
        { path: '/', replace: '/management' },
        { path: 'app', alias: '/app/kr' },
        { path: 'tools/:name', replace: to => {jump(to)}}
    ]
```


### 其它建议

- 设置404页面
- 设置跳转页面
- 命名视图：有时候想同时（同级）展示多个视图
- 视图切换：视图状态保存


## 权限控制

> 通过路由钩子函数和元信息设置权限控制


```js
router.beforeEach((to, from, next) => {
    const needAuth = to.matched.some(
        record => record.meta.requiresAuth)
    if (needAuth) {
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