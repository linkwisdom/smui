# 输入型组件

> 输入型组件是接收用户数据输入（文本、数字、选择操作）的组件

```html
    <date-picker v-model="range"></date-picker>
    <city-picker v-model="cityInfo"></city-picker>
```

- 输入型组件的特点是组件是一个数据输入源
- 组件通过`emit`事件`input`，调用组件能够通过`v-model`指令进行数据绑定

```js
{
    data () {
        return {
            value: 0
        }
    },
    methods: {
        changeValue () {
            this.$emit('input', this.value)
        }
    }
}

```

- 采用`v-model`方式绑定输入组件，最大程度减少重复回调行数的开发

```html
<data-picker #data-change="changeRawValue"></data-picker>
```



