## 路由的设计

路由参数结构

> /path/subPath/:params?query



- 建议1：path/subPath/能够对应功能模块
    - 原因：文件的目录结构清晰，路由规则建档明确

- 建议2：如果项目导航结构是多层级，建议使用子路由规则
    - 如果模块少，不存在多级导航，可以用一级别路由
    - 多层级目录；多层级导航；动态加载情况下使用子路由
    
- 建议3：params慎用，除非是为了动态路由/切换状态
    - params容易与子路由冲突
    - params扩展不方便，而且容易造成path过长
    - params不适合参数更新频繁情况
    - params不如query参数语义不明确

- 建议4: query与store建立映射关系






### 其它建议

- 设置404页面
- 设置跳转页面
- router
- 命名视图：有时候想同时（同级）展示多个视图
- 视图切换：视图状态保存


## 权限控制：通过路由钩子函数和元信息设置权限控制



```eee

```



```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
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