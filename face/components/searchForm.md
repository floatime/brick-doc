# searchForm组件

>地址
>/components/searchForm/bigSearchForm.js

>引用
>import bigSearchForm from '/components/searchForm/bigSearchForm.js'

>注册
>components 对象内注册 bigSearchForm 和引用时起的名字相同

>使用

```js
// html
<big-search-form :config="config" :render="render" :behavior="behavior" :event="event"></big-search-form>
// js
data(){
    return{
        config: {
                // 查看下方form控件详情
            formArray: [
                {
                    type: 'input',
                    config: {
                        // type:'textarea',
                        name: 'name',//表单控件绑定值
                        label: '姓名',//表单控件label
                        defaultValue: '王某',//表单控件初始化默认值
                        placeholder: '请输入',//表单控件占位文本
                        // maxlength: 100,//类型 Number 最大输入长度
                        // minlength: 10,//类型 Number 最小输入长度
                    },
                    rules: [
                        {
                            type: 'req',
                            msg: '提示',
                            tri: 'blur'
                        }
                    ],
                    render: {
                        //labelWidth: '120px',
                        span: 12,
                    },
                    event: {
                        change: function (value) {
                            console.log(value)
                        },
                        enter: function () {
                            console.log(99999)
                        }
                    },
                    behavior: {
                        readonly: false,
                        disabled: false,
                        clearable: true,
                        hide: false,
                    }
                },
            ],
         },
        render: {
            size: 'small',
            labelWidth: '80px',
            labelPosition: 'left'
        },
        event: {},
        behavior: {}
    }
    
}
```

## 属性
| 参数名   | 数据类型 | 说明         | 默认值 |
| -------- | -------- | ------------ | ------ |
| config   | Object   | 基本数据     | 必填   |
| render   | Object   | 控制渲染样式 | {}     |
| event    | Object   | 方法         | {}     |
| behavior | Object   | 行为         | {}     |

### config

```js
config: {
    // form渲染控件的数据 详细查看下方form控件详情
    formArray: [
        // 控件数据
        {
            
        }
    ],
},
```

### render

```js
render: {
    // 控件尺寸 (默认为small) enum类型 large || default || small
    size: 'small',
    // label 宽
    labelWidth: '80px',
    // label 方向（默认为left）enum类型 left || right || top
    labelPosition: 'left',
    // 占据的份数 表单被分为24份 默认值为12
    span:12,
    // 是否可折叠表单 默认值为true
    showFold:true,
    // 表单必填验证星号的位置。默认值为left 可选值 left 和 right
    requireAsteriskPosition:'right',
},
```

## 事件

| 事件名 | 说明 | 参数 |
| ------ | ---- | ---- |
| —— | —— | —— |

## 插槽

| 插槽名 | 说明 | 参数 |
| ------ | ---- | ---- |
| form-tail | 操作 | —— |

## 组件属性
| 参数名   | 数据类型 | 说明         |
| -------- | -------- | ------------ |
| isChange   | Boolean  | 表单内数据是否被修改过（重置表单后会重新监听）   |
| formData   | Object  | 表单对象   |

## 方法

| 方法名 | 说明 | 参数 |
| ------ | ---- | ---- |
| verificationForm | 获取验证结果 | 返回一个Promise对象 then为成功 catch为失败 失败参数为失败的表单数组 |
| resetForm | 重置表单 |  |

### form控件详情

### 格式

```js
widget:{
    // 控件类型(参考下面类型)默认为input
    type:'',
    // 控件的基本数据 参考下面具体类型的内容
    config:{},
    // 控件的验证规则 参考下面验证规则的内容
    rules:[],
    // 控件的具体样式
    render:{},
    // 控件的触发方法
    event:{},
    // 控件拥有的行为
    behavior:'',
}
```

### input（输入框）

```js
{
    // 类型 输入框(可以省略默认为input)
    type: 'input',
    config: {
        // 输入框类型(默认为text 可以省略) 'text' || 'textarea' || 'password' 
        type:'textarea',
        // 输入框绑定表单对象的值
        name: 'name',
        // 表单label渲染的值
        label: '姓名',//表单控件label
       	// 表单控件初始化默认值
        defaultValue: '王某',
        // 表单控件占位文本
        placeholder: '请输入',
        // 最大输入长度 类型为Number类型
        maxlength: 100,
        // 最小输入长度 类型为Number类型
        minlength: 10,
    },
    // 参考验证规则
    rules: [],
    render: {
        // 表单label 宽
        labelWidth: '120px',
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
{
    // 类型 数字输入框
    type: 'input-number',
    config: {
        // 输入框绑定表单对象的值
        name: 'age',
        // 表单label渲染的值
        label: '年龄',
        // 表单控件初始化默认值
        defaultValue: 18,
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
    // 参考验证规则
    rules: [],
    render: {
        // 表单label 宽
        labelWidth: '120px',
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
{
    // 类型 下拉选择框
    type: 'select',
    config: {
        // 输入框绑定表单对象的值
        name: 'sex',
        // 表单label渲染的值
        label: '性别',
        // 表单控件初始化默认值
        defaultValue: '1',
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
    // 参考验证规则
    rules: [],
    render: {
        // 表单label 宽
        labelWidth: '120px',
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

### cascader（多级联动选择框）

```js
{
    // 类型 多级联动选择框
    type: 'cascader',
    config: {
        // 输入框绑定表单对象的值
        name: 'occupation',
        // 表单label渲染的值
        label: '职业',
        // 表单控件初始化默认值
        defaultValue: '1-2',
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
    // 参考验证规则
    rules: [],
    render: {
        // 表单label 宽
        labelWidth: '120px',
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
{
    type: 'tree-select',
    config: {
        // 输入框绑定表单对象的值
        name: 'occupation',
        // 表单label渲染的值
        label: '职业',
        // 表单控件初始化默认值
        defaultValue: '1-2',
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
    // 参考验证规则
    rules: [],
    render: {
        // 表单label 宽
        labelWidth: '120px',
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
{
    // 类型 日期选择框
    type: 'date',
    config: {
        // 日期类型
        // year（年），month（月），week（周），date（日期），datetime（日期加时间）默认值为字符串 例如’2001-10-10’
		// datetimerange（日期时间区间），daterange（日期区间），monthrange（月区间）默认值为数组 例如:［’2001-10-10’,’2021-10-10’］
        type: 'daterange',
        // 输入框绑定表单对象的值
        name: 'date',
        // 表单label渲染的值
        label: '出生日期',
        // 表单控件初始化默认值
        defaultValue: [],
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
    rules: [],
    render: {
        // 表单label 宽
        labelWidth: '120px',
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



## 验证规则

> type：req（必填），len（字符串长度），phone（手机号），email（电子邮箱），idCard（身份证号），landline（固定电话），reg（正则验证），custom（自定义规则），num（数字）

?>tri：'change'(改变触发) or 'blur'(失去焦点)触发方式 默认值blur	msg：验证失败提示

> req（必填）

```javascript
{
    type:'req',
    msg:'提示',
    tri:'blur'
}
```

> len（字符串长度）

```javascript
{
    type:'len',
    msg:'提示',
    tri:'blur',
    min:5,//最小长度
    max:10,//最大长度
}
```

>phone（手机号）

```js
{
    type:'phone',
    msg:'提示',
    tri:'blur'
}
```

>email（电子邮箱）

```js
{
    type:'email',
    msg:'提示',
    tri:'blur'
}
```

>idCard（身份证号）

```js
{
    type:'idCard',
    msg:'提示',
    tri:'blur'
}
```

> landline（固定电话）

```js
{
    type:'landline',
    msg:'提示',
    tri:'blur'
}
```

> reg（正则验证）

```js
{
    type:'reg',
    msg:'提示',
    tri:'blur',
    reg:'', //正则表达式
}
```

> custom（自定义规则）

```js
{
    type:'custom',
    msg:'提示',
    tri:'blur',
    //自定义函数(rule, value, callback)接收四个参数  value(当前值) callback()回调函数(返回空为通过,字符串为验证不通过,字符串为校验失败提示)
    met:function(rule, value, callback){
        callback()
    }
}
```

>num（数字）

```js
{
    type:'num',
    msg:'提示',
    tri:'blur',
    max:8,//最大值（数字类型）
    min:2,//最小值（数字类型）
}
```

