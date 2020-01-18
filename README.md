# evan-radio
uniapp单选框组件，支持双向数据绑定，支持自定义icon   
测试过微信小程序、app（包括nvue模式）、h5

### 1.基本用法

```
<template>
    <evan-radio v-model="baseValue" label="base1">基础用法1</evan-radio>
    <evan-radio v-model="baseValue" label="base2">基础用法2</evan-radio>
    <evan-radio-group v-model="color">
        <evan-radio ﻿v-for="item in colorList" :key="item.value" :label="item.value">{{item.label}}</evan-radio>
    </evan-radio-group>
</template>
<script>
export default{
    data(){
        return{
            baseValue:'base1',
            color: 'red',
            colorList: [{
                    label: '红色',
                    value: 'red'
                },
                {
                    label: '绿色',
                    value: 'green'
                },
                {
                    label: '蓝色',
                    value: 'blue'
                },
                {
                    label: '粉色',
                    value: 'pink'
                },
                {
                    label: '黑色',
                    value: 'black'
                }
            ]
        }
    }
}
</script>
```

### 2.更多用法
参考[github demo](https://github.com/EvanMaFYH/evan-radio)中的用法

### 3.注意点

#### 1.nvue模式下radio的文本需要包裹在text组件中
```
<evan-radio v-model="value">单选框1</evan-radio>
<evan-radio v-model="value"><text class="your-class">单选框2</text></evan-radio>
```
#### 2.通过slot方式自定义图标时两种状态如果是某个图标的显示与隐藏，在外层套一个view，不然在h5下有问题
```
<evan-radio v-model="value">
   ﻿<text>完全自定义图标</text>
    <template slot="icon">
        <uni-icons type="chat" size="20" :color="value==='value1'?'#108ee9':'#c8c9cc'"></uni-icons>
    </template>
</evan-radio>

<evan-radio v-model="value">
    <text>完全自定义图标</text>
    <template slot="icon">
        <view>
            <uni-icons v-if="value==='value1'" type="checkmarkempty" size="30" color="#108ee9"></uni-icons>
        </view>
    </template>
</evan-radio>
```

### evan-radio props
| 参数           | 说明            | 类型    | 可选值     | 默认值  |    
| :-------------------- | :------------------------------ | :---------- | :-------- | :--- |  
| v-model | 当前选中的label值 | string/number/boolean | - | - |
| shape | 在非自定icon模式下图标的形状 | string | round/square | round |
| label | 与v-model对应的值 | string/number/boolean | - | - |
| disabled | 是否禁用 | boolean | - | false |
| primary-color | 主题色 | string | - | #108ee9 |
| icon | 自定义图标，使用uni-icons的图标 | string | - | - |
| icon-size | 图标的大小 | number | - | 20 |
| title-style | 单选框的文本样式 | object | - | - |
| prevent-click | 阻止内部的点击事件，只在自定义样式由外部触发select事件时设置为true | boolean | - | false |

### evan-radio methods
| 方法名   | 说明       | 参数     |   
| :--------------- | :------------------------------------ | :-------|
| select | 选择当前radio，一般配合prevent-click属性使用来自定义样式 | - |

### evan-radio slot
| name | 说明 |
| :--- | :---------------- |
| icon | evan-radio自定义icon，如果是用的图片等显示不居中，将图片的display设为block |

### evan-radio-group props   
| 参数           | 说明            | 类型    | 可选值     | 默认值  |    
| :------------- | :------------------------------ | :------ | :----- | :--- |  
| v-model | 选中项的label | string/number/boolean | - | - |
| disabled | 是否禁用 | boolean | - | false |

### evan-radio-group events
| name | 说明 | 回调参数 |
| :--- | :---------------- | ------------------|
| change | 选中的值发生变化 | 新的选中的值 |
