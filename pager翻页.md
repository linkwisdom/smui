## 翻页组件 Pager

> 基础翻页功能


### 基础功能

> Pager 支持功能

- 前端翻页：传入数组自动分页
- 后端翻页：每次请求后端更新翻页信息
- 定制页面大小
- 跳转制定页面

### 参数: params

```json
{
    "currentPage": 1, // 当前页号 >= 1
    "pageSize": 20,  // 单页记录数量
    "totalSize": 100, //  总记录数量
    "pageCount": 5,   // 总页数
    "sizeOption": [20, 50, 100] // 单页记录数量选择项，如果只有一项不现实选择
}
```

### 事件: Events

* page-change
    - e.currentPage 当前页号变化

* size-change
    - e.pageSize 页面大小变化

* list-change
    - e.list 列表数据发生变化, 只有自定义分页才会触发

## 用例: usage

```html
<div>
<rich-table :params="tableConf"
        @selected-changed="changeSelected" v-ref:table>
</rich-table>
<pager
    :params="pageInfo"
    @page-change="changePage"
    @size-change="changePage"
    @list-change="changeList">
</pager>
</div>
```

```js
import RichTable from 'smui/RichTable'
import DatePicker from 'smui/DatePicker'
import Pager from 'smui/Pager'
import Dialog from 'smui/Dialog'

export default {
    data () {
        return {
            selected: [],
            tableConf: tableConf,
            pageInfo: {
                pageSize: 20,
                items: dataList,
                sizeOptions: [10,20,50],
                currentPage: 1
            }
        }
    },
    methods: {
        changeList (e) {
            Object.assign(this.pageInfo, e.data)
            if (this.$refs.table) {
                this.$refs.table.items = e.data.list;
            }
        },
        changePage (e) {
            Object.assign(this.pageInfo, e.data)
            if (this.$refs.table) {
                this.$refs.table.setSelected([]);
            }
        },
        changeSelected (e) {
            this.selected = e.data
            // 也可以直接双向绑定batch.disabled属性，但是不建议这么做
            this.$refs.batch.disabled = e.data.length === 0
        }
    },
    components: {
        DropList,
        RichTable,
        Pager,
        DatePicker
    }
}
```

