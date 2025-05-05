# page组件


>地址
>/components/page/bigPage.js

>引用
>import bigPage from '/components/form/bigPage.js'

>注册
>components 对象内注册 bigPage 和引用时起的名字相同

>使用
```js
// html
<big-page :config="config" :render="render"></big-page>
// js
data(){
    return{
        config: {
            paginationDetail: {
                config: {
                    pageSize: 10,
                    pageNum: 1,
                    total: 10,
                }
            },
            tableDetail: {
                config: {
                    tableData: [
                        {id: '1', name: '姓名', age: '122'},
                      
                    ],
                    tableLabel: [
                       {
                            label: '姓名',
                            prop: 'name',
                        }, {
                            label: '年龄',
                            prop: 'age',
                        }
                    ],
                    operation: [
                        {
                            name: '详情',
                            icon: 'Place',
                            method: this.btnClick
                        }
                    ],
                },
                event: {}
            },
            operationDetail: {
                config: [
                    {
                        value: '搜索',
                        type: 'primary',
                        plain: true,
                        beforeIcon: 'Search',
                        endIcon: 'Memo',
                        method: function(){},
                        width: '100px'
                    }
                ]
            },
            formDetail: {
                config: {
                    formArray: [
                        {
                            config: {
                                // type:'textarea',
                                name: 'name',//表单控件绑定值
                                label: '姓名',//表单控件label
                                defaultValue: '王某',//表单控件初始化默认值
                                placeholder: '请输入',//表单控件占位文本
                            },
                        },
                    ],
                },
            },
        },
        render: {
            // 组件最大高度
            maxHeight: 999,
        }
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

| 参数名   | 数据类型 | 说明         | 地址 |
| -------- | -------- | ------------ | ------ |
| paginationDetail   | Object   | 分页信息     |  [paging](face/components/paging)  |
| tableDetail   | Object   | 表格信息           |     [table](face/components/table)        |
| operationDetail    | Object   | 操作台信息   |   [operation](face/components/operation)   |
| formDetail | Object   | 搜索表单组件          |    [form](face/components/searchForm)  |

```js
config: {
   //分页信息 请查看[paging组件] 如不需要可以不传
    paginationDetail:{}
    // 表格信息 请查看[table组件] 如不需要可以不传
	tableDetail:{}
    // 操作台信息 请查看[operation组件] 如不需要可以不传
	operationDetail:{}
    // 搜索表单组件 请查看(form组件) 如不需要可以不传
	formDetail:{}
}
```



## 事件  

> 有且只包含（ paging组件，table组件，operation组件 ，form组件）中的事件

## 方法

| 方法名              | 说明               | 参数            |
|------------------| ------------------ | --------------- |
| setFormData      | 设置表单对象值 | 表单对象 |
| getFormData      | 获取表单对象 | —— |
| verificationForm | 获取form的验证对象 | 返回promise对象 |
| resetForm        | 重置表单           | ——              |
| updateFormData   | 修改表单值         | key , value    |
| getSelectionRows | 获取表格当前已选择项         |     |

