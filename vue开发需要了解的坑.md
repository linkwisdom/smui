## 属性监听

1. 变化的定义，对象引用变化或基础类型的数值变化
2. 数组的变化定义：length属性发生变化，或者数组引用发生变
3. 深度监听，并不包括数组对象成员

4. 防止循环更新


```js
{
    data () {
        return {
            message: '',
            info: ''
        }
    },
    watch : {
        message (val) {
            this.info = val + '/'
        },
        info (val) {
            this.message = val + '|'
        }
    }
}
```

## 计算属性



* 计算属性更新的条件是通过依赖分析，如果依赖的变量没有发生变化，则不会重新计算
* 因为计算属性包含了变量声明，因此至少有一次初始化过程，但是初始化时机是在挂载\(mounted\)实例之前，因此第一次计算函数要注意容错处理，比如防止访问不存在的变量或者引用对象
* 变量依赖是个复杂而且可能违法直觉的过程，

### 变化检测

Vue 不允许在已经创建的实例上动态添加新的根级响应式属性(root-level reactive property)。然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上：

```js
Vue.set(vm.someObject, 'b', 2)
```

您还可以使用 vm.$set 实例方法，这也是全局 Vue.set 方法的别名：

```js

this.$set(this.someObject,'b',2)
```

有时你想向已有对象上添加一些属性，例如使用 Object.assign() 或 _.extend() 方法来添加属性。但是，添加到对象上的新属性不会触发更新。在这种情况下可以创建一个新的对象，让它包含原对象的属性和新的属性：

```js
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

### 声明响应式属性

由于 Vue 不允许动态添加根级响应式属性，所以你必须在初始化实例前声明根级响应式属性，哪怕只是一个空值:

```js
var vm = new Vue({
  data: {
    // 声明 message 为一个空值字符串
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// 之后设置 `message` 
vm.message = 'Hello!'
```

如果你在 data 选项中未声明 message，Vue 将警告你渲染函数在试图访问的属性不存在。


### 异步更新队列

可能你还没有注意到，Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会一次推入到队列中。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际（已去重的）工作。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MutationObserver，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。

例如，当你设置 vm.someData = 'new value' ，该组件不会立即重新渲染。当刷新队列时，组件会在事件循环队列清空时的下一个“tick”更新。多数情况我们不需要关心这个过程，但是如果你想在 DOM 状态更新后做点什么，这就可能会有些棘手。虽然 Vue.js 通常鼓励开发人员沿着“数据驱动”的方式思考，避免直接接触 DOM，但是有时我们确实要这么做。为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用 Vue.nextTick(callback) 。这样回调函数在 DOM 更新完成后就会调用。例如：


```js
this.$nextTick(function () {
  console.log(this.$el.textContent) // => '更新完成'
})

```






