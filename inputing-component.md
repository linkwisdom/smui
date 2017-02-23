# 输入型组件

> 输入型组件是接收用户数据输入（文本、数字、选择操作）的组件

```html
    <date-picker v-model="range"></date-picker>
    <city-picker v-model="cityInfo"></city-picker>
```

- 输入型组件的特点是组件是一个数据输入源

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

