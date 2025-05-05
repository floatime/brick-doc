# operation组件


>地址
>/components/operation/bigOperation.js

>引用
>import bigOperation from '/components/operation/bigOperation.js'

>注册
>components 对象内注册 bigOperation 和引用时起的名字相同

>使用

```js
// html
<big-operation :config="config"></big-operation>
// js
data(){
    return{
        config: [
            {
                value: '搜索',
                type: 'primary',
                plain: true,
                beforeIcon: 'Search',
                method: function(){},
                width: '100px'
            }, {
                value: '重置', 
                type: 'success',
                plain: true,
                method: function(){},
            }
        ]
    }
}

```
![button-type](../../assets/image/face/page-style/operation.png)
## 属性
| 参数名   | 数据类型 | 说明             | 默认值 |
| -------- | -------- | ---------------- | ------ |
| config   | Arra     | 控制按钮基本数据 | 必填   |
| render   | Object   | 控制渲染样式     | {}     |
| event    | Object   | 方法             | {}     |
| behavior | Object   | 行为             | {}     |

```js
config:[{
    // 按钮渲染内容
    value: '搜索',
    // 按钮类型 (默认default) enum类型 primary || success || info || warning || danger
    type: 'primary',
    // 是否为朴素按钮
    plain: true,
    // 是否为圆角按钮
    round: true,
    // 按钮前图标
    beforeIcon: 'Search',
    // 点击按钮触发
    method: function(){},
    // 按钮宽度
    width: '100px'
    // 按钮尺寸 (默认为small) enum类型 large || default || small
    size: 'small'
}]
```

![button-type](../../assets\image\face\button-type.png)