# SMUI开发必知必会

## SMUI组件设计细节

* 所有ui组件继承基础的UI类型，保证所有UI元素有相同的初始化方法，如prepare、setOptions和共有的class绑定等特性。
* 参数化属性：所有组件采用简化方式，对外传入统一用params参数，不用纯粹的data或者props作为参数传递方式

  * props.params是组件接收参数的建议的唯一方式
  * params被深度监听，如果有变化自动调用setOptions方法

* 生命周期兼容：为了让组件能够在vue1.0和vue2.0中平滑过渡，使用created和mounted两个钩子，让所有ui组件都会有相同的初始化方法


## SMUI组件HACK方法

* 容器组件如何穿透事件？

  * 由于vue2.0没有dispatch方法，为了能够穿透容器，容器组件只需要劫持所有子组件的emit方法并且对外传递即可。emit参数的target属性可知子组件的引用。

* 如何为所有组件扩展方法

  * Vue.prototype.extFunction 可以动态为所有Vue组件扩展动态行为
  * **UI.prototype.extFunction 可以动态为所有UI组件扩展动态行为**


* 如何阻止频繁的数据刷新
  * 如果一条远程ajax请求包含多个参数，每个参数发生变化都需要更新列表数据，在一100ms内可能造成多次请求，其实只有最后一条请求是合法合理的
  * 在翻页器中，pageSize, pageCount和totalSize几个参数是相互制约的，更新任何参数都需要调用getPages函数，如果顺序不对，可能造成获取的翻页列表是错误的。
  * 使用延迟更新的方法能够避免以上两个问题，比如用setTimeout延迟，在loadash和underscore中有debounce方法可以直接使用。

* **所以要让组件的`v-model`生效，它必须**：

  * 接受一个`value`属性
  * 在有新的 value 时触发`input`事件




