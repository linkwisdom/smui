

## 属性监听

1. 变化的定义，对象引用变化或基础类型的数值变化
2. 数组的变化定义：length属性发生变化，或者数组引用发生变
3. 深度监听，并不包括数组对象成员

4. 防止循环更新


```
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



## 表达式





