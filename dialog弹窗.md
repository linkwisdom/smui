## 弹窗 Dialog

> Dialog 弹窗是一个信息容器窗口，不仅能够展现文本类信息，还可以将复杂的组件包含在窗体内。

## 使用说明

- Dialog
    - show(`option`)
    - showModal(`option`)
    - close()
    - hide()
- InlineDialog 
    - slot.title
    - slot.content
    - slot.footer
- Message
    - alert(`msg`, callback) 消息警告
    - comfirm(`msg`, callback, quit) 确认对话框
    - prompt(`msg`, callback) 输入对话框
    - popup(`msg`, callback) 弹出消息（页边）
    - toast(`msg`) 提醒消息 （闪出消息）

## 方法 methods

* 显示弹窗 show

> 窗口位置和大小受到寄生元素限制


```js
var dialog = new Dialog();

// 无显示参数
// dialog.show();

// 配置显示参数
dialog.show({
    title: '标题',
    content: '内容部分',
    clazz: ['mydialog'], // 自定义窗口类，方便样式扩展
    width: 400, // 设置窗口大小
    height: 500,
    top: 50 // 设置窗口位置
})

dialog.$on('close', function () {
    // 窗口关闭
})

dialog.$on('ensure', function () {
    // 点击确定触发的事件
});

```

* 包含子组件的弹窗

```js

var Form = requrie('./Form.vue')

var dialog = new Dialog({
    data: function () {
        return {
            data: {
                item: {}
            }
        }
    },
    components: {
        ContentView: Form
    }
});

dialog.$on('finished', function () {
    // finished 是Form 组件透传出得事件
    dialog.close();
});

// 透传所有事件，Vue暂时还不支持
dialog.$on('*', function () {

});

```


* 使用InlineDialog

> InlineDialog 使用内容分发方式，使得父子组件通信更加方便

```html
<template>
<div>
    <h1>使用InlineDialog</h1>
    <inline-dialog v-ref:"dialog">
        <div slot="title">标题</div>
        <div slot="content">
            <form>
                <input type="text" v-model="name" name="name">
            </form>
        </div>
        <div slot="footer">
            <button @click="submit">确定</button>
        </div>
    </inline-dialog>
</div>
</template>
<script type="text/javascript">
import InlineDialog from 'smui/InlineDialog'

export default {
    components: {InlineDialog},
    methods: {
        submit () {
            // 子组件直接可以调用submit
            this.$refs.dialog.close()
        }
    }
}

</script>

```

* 模态显示弹窗 showModal

> 窗口位置、大小自适应可视区域


```js
var dialog = new Dialog();
dialog.showModal();

```