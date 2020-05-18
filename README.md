# evan-radio
uniapp单选框组件，支持双向数据绑定，支持自定义icon   
测试过微信小程序、app（包括nvue模式）、h5   
试过支付宝小程序暂不支持   
其他未说明平台也可理解为暂不支持

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

#### 3.EvanRadioPopup中使用到了解构插槽，目前uniapp似乎在这一块问题还比较多，如果要在同一个页面使用多个EvanRadioPopup组件时记得要import多个，不然目前slot中的内容的样式除了第一个都会被最后一个影响，预计官方在下个版本修复，参考[这里](https://github.com/dcloudio/uni-app/issues/1662)
```
<evan-radio-popup></evan-radio-popup>
<evan-radio-popup2></evan-radio-popup2>
<evan-radio-popup3></evan-radio-popup3>

import EvanRadioPopup from '@/components/evan-checkbox/evan-checkbox-popup.vue'
import EvanRadioPopup2 from '@/components/evan-checkbox/evan-checkbox-popup.vue'
import EvanRadioPopup3 from '@/components/evan-checkbox/evan-checkbox-popup.vue'
export default{
    components:{
        EvanRadioPopup,
        EvanRadioPopup2,
        EvanRadioPopup3
    }
}
```

#### 4.如果层级出现问题去调一下uniPopup组件的层级试试（uniPopup组件推荐将popup写在其他元素后面，但是目前是在组件中的，因此如果嵌套比较深可能无法弹出）   

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
| clearable | 选中的选项再次点击能否取消选中 | boolean | - | false |

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

### evan-radio-popup props   
| 参数           | 说明            | 类型    | 可选值     | 默认值  |    
| :------------- | :------------------------------ | :------ | :----- | :--- |  
| v-model | 选中项的label | string/number/boolean | - | - |
| options | 选项数组 | array | - | - |
| optionLabel | 选项数组中显示内容对应键名 | string | - | label |
| optionValue | 选项数组中value内容对应键名 | string | - | value |
| primary-color | 主题色 | string | - | #108ee9 |
| cancelText | 取消按钮的文字 | string | - | 取消 |
| confirmText | 确定按钮的文字 | string | - | 确定 |
| title | 标题的文字 | string | - | 请选择 |
| clearable | 选中的选项再次点击能否取消选中 | boolean | - | false |
| maskClick | 点击遮罩层是否关闭 | boolean | - | true |

### evan-radio-popup slot
| name | 说明 |
| :--- | :---------------- |
| - | 触发选项弹窗的元素 |
| trigger | 触发选项弹窗的元素，会返回给父组件当前选中项的显示值 |

### evan-radio-popup events
| name | 说明 | 回调参数 |
| :--- | :---------------- | ------------------|
| confirm | 点击确定的回调 | 点击确定时选中值的value数组 |
| objConfirm | 点击确定的回调 | 点击确定时选中的选项数组 |
| cancel | 取消关闭弹窗时的回调（包括点击遮罩层关闭）| 关闭时选中值的value数组 |
