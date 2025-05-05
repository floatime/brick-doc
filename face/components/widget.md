# widget组件

>地址
>/components/widget

>引用
>import bigInput from '/components/widget/bigInput.js'
>
>import bigInputNumber from '/components/widget/bigInputNumber.js'
>
>import bigCascader from '/components/widget/bigCascader.js'
>
>import bigDatePinker from '/components/widget/bigDatePinker.js'
>
>import bigSelect from '/components/widget/bigSelect .js'
>
>import bigRadio from '/components/widget/bigRadio.js'
>
>import bigTreeSelect from '/components/widget/bigTreeSelect.js'

>注册
>components 对象内注册

>使用 其他使用与（input 雷同）

```js
<big-input 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-input>
```

## 属性

| 参数名   | 数据类型 | 说明         | 默认值 |
| -------- | -------- | ------------ | ------ |
| config   | Object   | 基本数据     | 必填   |
| render   | Object   | 控制渲染样式 | {}     |
| event    | Object   | 方法         | {}     |
| behavior | Object   | 行为         | {}     |

## 类型

| 类型名         | 类型说明           | 地址                                                         |
| -------------- | ------------------ | ------------------------------------------------------------ |
| bigInput       | 输入框组件         | [地址](face/components/widget?id=input（输入框）)            |
| bigInputNumber | 数字输入框或步进器 | [地址](face/components/widget?id=input-number（数字输入框）) |
| bigSelect      | 选择框             | [地址](face/components/widget?id=select（下拉选择框）)       |
| bigRadio       | 单选框             | [地址](face/components/widget?id=radio（单选框）)            |
| bigCascader    | 多级联动选择框     | [地址](face/components/widget?id=cascader（多级联动选择框）) |
| bigTreeSelect  | 树选择器           | [地址](face/components/widget?id=tree-select(树下拉框))      |
| bigDatePinker  | 日期输入框选择框   | [地址](face/components/widget?id=date（日期选择框）)         |


### input（输入框）

```js
<big-input 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-input>

import bigInput from '/components/widget/bigInput.js'
```

```js
{
    config: {
        // 输入框类型(默认为text 可以省略) 'text' || 'textarea' || 'password' 
        type:'textarea',
        // 表单控件占位文本
        placeholder: '请输入',
        // 最大输入长度 类型为Number类型
        maxlength: 100,
        // 最小输入长度 类型为Number类型
        minlength: 10,
    },
    render: {
       
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
        // 输入框回车触发
        enter: function () {
            console.log('回车触发')
        }
    },
    behavior: {
        // 只读 默认为false
        readonly: false,
        // 禁用 默认为false
        disabled: false,
        // 可清空 默认为false
        clearable: true,
        // 表单隐藏 默认为flase
        hide: false,
    }
}
```

### input-number（数字输入框）
```js
<big-input-number 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-input-number>

import bigInputNumber from '/components/widget/bigInputNumber.js'
```

```js
{
    config: {
        // 表单控件占位文本
        placeholder: '请输入',
        // 数值精度 类型 Number
		precision: 2,
        // 计数器步长 配合 controls 使用
		step: 1,
        // 设置计数器允许的最大值
		max: 100,
        // 设置计数器允许的最小值
		min: 90,
    },
    render: {
        
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
        // 输入框回车触发
        enter: function () {
            console.log('回车触发')
        }
    },
    behavior: {
        // 只读 默认为false
        readonly: false,
        // 禁用 默认为false
        disabled: false,
        // 可清空 默认为false
        clearable: true,
        // 表单隐藏 默认为flase
        hide: false,
        // 是否使用控制按钮
        controls: false,
    },
},
```

### select（下拉选择框）
```js
<big-select 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-select>

import bigSelect from '/components/widget/bigSelect.js'
```

```js
{
    config: {
        // 表单控件占位文本
        placeholder: '请选择',
        // 下拉列表的详细数据
        optionInfo: {
            props: {
                // 作为渲染的key 不可重复
                key: 'id',
                // 选中后表单绑定的值,若不设置则默认为 value
                value: 'id',
                // 选中后控件渲染的内容,若不设置则默认与value相同
                label: 'name',
                // 指定选项的禁用为选项对象的某个属性值 默认值'disabled'
                disabled: 'disabled',
            },
            // 选项的列表
            options: [
                {
                    id: '1',
                    name: '女',
                }, {
                    id: '0',
                    name: '男',
                }
            ]
        }
    },
    render: {
       
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
    },
    behavior: {
        // 只读 默认为false
        readonly: false,
        // 禁用 默认为false
        disabled: false,
        // 可清空 默认为false
        clearable: true,
        // 表单隐藏 默认为flase
        hide: false,
        // 多选 默认为flase
        multiple: false,
        // 本地筛选 默认为flase
        filterable: false,
        // 是否允许用户创建新条目 默认为flase
        allowCreate: false,
    },
}
```

### radio（单选框）
```js
<big-radio 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-radio>

import bigRadio from '/components/widget/bigRadio.js'
```

```js
{
    config: {
        // 选项配置详情
        optionInfo: {
            props: {
                // 选中后表单绑定的值
                value: 'id',
                // 控件渲染的内容
                label: 'name',
                // 指定选项的禁用为选项对象的某个属性值 默认值'disabled'
                disabled: 'disabled',
            },
            // 选项的列表
            options: [
                {
                    id: '1',
                    name: '女',
                }, {
                    id: '0',
                    name: '男',
                }
            ]
        }
    },
    render: {
       
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
    },
    behavior: {
        // 禁用 默认为false
        disabled: false,
    },
}
```

### checkbox（多选框）
```js
<big-checkbox
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-checkbox>

import bigCheckbox from '/components/widget/bigCheckbox.js'
```

```js
{
    config: {
        // 选项配置详情
        optionInfo: {
            props: {
                // 选中后表单绑定的值
                value: 'id',
                // 控件渲染的内容
                label: 'name',
                // 指定选项的禁用为选项对象的某个属性值 默认值'disabled'
                disabled: 'disabled',
            },
            // 选项的列表
            options: [
                {
                    id: '1',
                    name: '女',
                }, {
                    id: '0',
                    name: '男',
                }
            ]
        }
    },
    render: {
       
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
    },
    behavior: {
        // 禁用 默认为false
        disabled: false,
    },
}
```

### cascader（多级联动选择框）
```js
<big-cascader 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-cascader>

import bigCascader from '/components/widget/bigCascader.js'
```

```js
{
    config: {
        // 表单控件占位文本
        placeholder: '请选择',
        // 下拉列表的详细数据
        optionInfo: {
            props: {
               	// 选中后表单绑定的值,若不设置则默认为value
                value: 'id',
                // 选中后控件渲染的内容,若不设置则默认与value相同
                label: 'name',
                // 指定选项的子选项为选项对象的某个属性值
                children: 'children',
                // 是否严格的遵守父子节点不互相关联
                checkStrictly: false,
                // 是否多选 默认为false
                multiple: false,
                // 在选中节点改变时，是否返回由该节点所在的各级菜单的值所组成的数组，若设置 false，则只返回该节点的值 默认值true
                emitPath: false,
                // 指定选项的禁用为选项对象的某个属性值 默认值'disabled'
                disabled: 'disabled'
            },
            // 选项的列表
            options: [
                {
                    id: '1',
                    name: '计算机',
                    disabled: false,//是否禁用
                    children: [
                        {
                            id: '1-1',
                            name: '软件',
                            disabled: true,
                        }, {
                            id: '1-2',
                            name: '硬件',
                            disabled: false,
                        },
                    ],

                }
            ]
        }
    },
    render: {
        
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
    },
    behavior: {
      	// 只读 默认为false
        readonly: false,
        // 禁用 默认为false
        disabled: false,
        // 可清空 默认为false
        clearable: true,
        // 表单隐藏 默认为flase
        hide: false,
        // 本地筛选 默认为flase
        filterable: false,
       	//显示完整路径，默认为true
        showAllLevels: false
    },
},
```

### tree-select(树下拉框)
```js
<big-tree-select 
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-tree-select>

import bigTreeSelect from '/components/widget/bigTreeSelect.js'
```

```js
{
    config: {
        // 表单控件占位文本
        placeholder: '请选择',
        // 下拉列表的详细数据
        optionInfo: {
            props: {
                // 选中后表单绑定的值,若不设置则默认为value
                value: 'id',
                // 选中后控件渲染的内容,若不设置则默认与value相同
                label: 'name',
                // 指定选项的子选项为选项对象的某个属性值
                children: 'children',
                // 是否严格的遵守父子节点不互相关联
                checkStrictly: false,
            },
            // 选项的列表
            options: [
                {
                    id: '1',
                    name: '计算机',
                    disabled: false,//是否禁用
                    children: [
                        {
                            id: '1-1',
                            name: '软件',
                            disabled: true,
                        }, {
                            id: '1-2',
                            name: '硬件',
                            disabled: false,
                        },
                    ],
                }
            ]
        }
    },
    render: {
      
       
    },
    event: {},
    behavior: {
        // 表单隐藏 默认为flase
        hide: false,
        // 禁用 默认为false
        disabled: false,
        // 可清空 默认为false
        clearable: true,
       	// 是否多选 默认为false
        multiple:false,
         // 本地筛选 默认为flase
        filterable:true,
        // 是否在第一次展开某个树节点后才渲染其子节点，默认为true
        renderAfterExpand:true,
        // 在显示复选框的情况下，是否严格的遵循父子不互相关联的做法，默认为 false
        checkStrictly:false,
    },
}
```

### date（日期选择框）
```js
<big-date-pinker  
	v-model="name"
	:config="config" 
	:render="render" 
	:behavior="behavior"
	:event="event">
</big-date>

import bigDatePinker  from '/components/widget/bigDatePinker .js'
```

```js 
{
    config: {
        // 日期类型
        // year（年），month（月），week（周），date（日期），datetime（日期加时间）默认值为字符串 例如’2001-10-10’
		// datetimerange（日期时间区间），daterange（日期区间），monthrange（月区间）默认值为数组 例如:［’2001-10-10’,’2021-10-10’］
        type: 'daterange',
        // 显示在输入框中的格式 完整的格式为'YYYY-MM-DD HH:mm:ss'
        format: 'YYYY-MM-DD HH:mm:ss ',
        // 绑定值的格式
        valueFormat: 'YYYY-MM-DD HH:mm:ss',
        //选择日期后的默认时间值。 如未指定则默认时间值为 (只取时分秒)（如果是datetime的话是 new Date(2000, 1, 1, 12, 0, 0)不是数组）
        defaultTime: [
            new Date(2000, 1, 1, 0, 0, 0),
            new Date(2000, 2, 1, 23, 59, 59),
        ]
    },
    render: {
       
    },
    event: {
        // 输入框内容修改触发
        change: function (value) {
            console.log(value)
        },
    },
    behavior: {
        // 只读 默认为false
        readonly: false,
        // 禁用 默认为false
        disabled: false,
        // 可清空 默认为false
        clearable: true,
        // 表单隐藏 默认为flase
        hide: false,
    },
},
```