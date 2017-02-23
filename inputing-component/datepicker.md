# DatePicker

> 日期选择组件

用法：

```html
<date-picker
    v-model="range"
    :params="rangeConf"
    @range-change="changeDateRange"
></date-picker>
```

说明：
    1. `v-model`绑定的是一个字符串取值，如`2012-02-12~2017-02-12`
    2. `params`参数是配置项目，配置参数如下
    
```json
{
    "show": true,
    "format": "YYYY-MM-DD",
    "info": "",
    "limitDates": 365,
    "value": "2017-02-16~2017-02-22",
    "rtype": "",
    "area": [
        "2015-08-23",
        "2017-02-23"
    ],
    "cut": {
        "today": 1,
        "lastDay": 1
    }
}

```