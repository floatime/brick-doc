# tree组件

> 地址
> /components/tree/bigTree.js

> 引用
> import bigTree from '/components/tree/bigTree.js'

> 注册
> components 对象内注册 bigTree 和引用时起的名字相同

> 使用

```js
// html 
<big-tree :config="config" :render="render" :event="event" :behavior="behavior"></big-tree>
// js
data(){
    return: {
	    config: {
			props: {
			    // 唯一索引不重复
                key: 'id'
                // 指定选项的值为选项对象的某个属性值
                value: 'id',
                // 指定选项标签为选项对象的某个属性值
                label: 'name',
                // 指定选项的子选项为选项对象的某个属性值
                children: 'children',
                // 是否严格的遵守父子节点不互相关联
                checkStrictly: false,
                // 是否严格的遵守父子节点不互相关联
                emitPath: false,
			},
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
		},
		render: {},
		event: {},
		behavior: {
            // 是否在第一次展开某个树节点后才渲染其子节点
            defaultExpandAll:false,
		    // 是否在点击节点的时候展开或者收缩节点， 默认值为 true，如果为 false，则只有点箭头图标的时候才会展开或者收缩节点。
		    expandOnClickNode: false,
		    // 节点是否可被选择
		    showCheckbox: false,
		    // 是否在点击节点的时候选中节点，默认值为 false，即只有在点击复选框时才会选中节点。
		    checkOnClickNode: false,
		    // 是否每次只打开一个同级树节点展开 默认值为false
		    accordion: false,
		    // 在显示复选框的情况下，是否严格的遵循父子不互相关联的做法，默认为 false
		    checkStrictly: false,
		    //是否可以搜索
		    showSearch: false,
		    //搜索条件
		    searchKey: ['id', 'name'],
		    // searchKey: 'id',
		},
	},	
}
```

![tree-style](../../assets/image/face/page-style/tree-style.jpg)

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
	//默认展开的节点的 key 的数组
	defaultExpandedKeys: ['1', '2'],
	//默认勾选的节点的 key 的数组
	defaultCheckedKeys: ['1'],
	props: {
        // 唯一索引不重复
		key: 'id',
		// 指定选项的值为选项对象的某个属性值
		value: 'id',
        // 指定选项标签为选项对象的某个属性值
		label: 'name',
        // 指定节点选择框是否禁用为节点对象的某个属性值
		disabled: 'disabled',
        // 指定选项的子选项为选项对象的某个属性值
		children: 'children',
        // 指定节点是否为叶子节点，仅在指定了 lazy 属性的情况下生效
         isLeaf:true
	},
	options: [
	    {
	        id: '1',
	        name: '计算机',
	        disabled: false,//是否禁用
            // 是否需要树节点后下拉详情功能
            isShowMore: true,
            // 树节点后下拉详情内容 需要开启behavior中的isShowMore为true才能使用
            dropdownList:[
                {
                    // 下拉按钮渲染的的名称
                    label: '修改',
                    // 	是否显示分隔符
                    divided: true,
                    // 是否禁用
                    disabled: true,
                    // 点击触发 返回参数为当前的树节点的数据
                    method: function (item) {
                        console.log(item)
                    }
                }
            ],
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
	],
    // 树节点后下拉详情内容 需要开启behavior中的isShowMore为true才能使用
    dropdownList: [
        {
            // 下拉按钮渲染的的名称
            label: '修改',
            // 	是否显示分隔符
            divided: true,
            // 是否禁用
            disabled: true,
            // 点击触发 返回参数为当前的树节点的数据
            method: function (item) {
                console.log(item)
            }
        }, {
            label: '排序',
            divided: true,
            disabled: true,
            method: function (item) {
                console.log(item)
            }
        }
    ],
},
```

### behavior

```js
behavior: {
    // 加载中 默认为false
    loading:true,
    // 是否需要树节点后下拉详情功能
    isShowMore:true,
    // 是否在第一次展开某个树节点后才渲染其子节点
    defaultExpandAll:false,
    // 是否在点击节点的时候展开或者收缩节点， 默认值为 true，如果为 false，则只有点箭头图标的时候才会展开或者收缩节点。
    expandOnClickNode: false,
    // 节点是否可被选择
    showCheckbox: false,
    // 是否在点击节点的时候选中节点，默认值为 false，即只有在点击复选框时才会选中节点。
    checkOnClickNode: false,
    // 是否每次只打开一个同级树节点展开 默认值为false
    accordion: false,
    // 在显示复选框的情况下，是否严格的遵循父子不互相关联的做法，默认为 false
    checkStrictly: false,
    // 是否开启搜索
    showSearch: false,
    // 是否搜索显示更多按钮
    showMore:false,
    // 搜索条件(数组为多条件,字符串为单条件)
    searchKey: ['id', 'name'],
    // searchKey: 'id',
        
    // 是否懒加载子节点，需与 loadNode 方法结合使用
    lazy:false,
},
```

### event

```js
event: {
    // 加载子节点数据的函数，lazy 为 true 时生效
	// node 当前点击节点信息
	// resolve 数据加载完成的回调(必须调用)
    loadNode:(node, resolve)=>{
    	let data=[]
    	resolve(data)
    }，
    
    // 隐藏更多操作
    // 返回值为false为隐藏更过按钮
    showMore:(data, node)=>{
        return false
    }，
    
    // 动态设置更多下拉内容
    // 返回值为config中的dropdownList格式
    getMoreDetail: (data, node)=>{
        return []
    }    
    
    
}
```



## 事件

| 事件名          | 说明                                   | 回调参数                                                         |
| --------------- | -------------------------------------- | ------------------------------------------------------------ |
| node-click       | 当节点被点击的时候触发                 | 四个参数：对应于节点点击的节点对象，TreeNode 的 `node` 属性, TreeNode和事件对象 |
| node-contextmenu | 当某一节点被鼠标右键点击时会触发该事件 | 共四个参数，依次为：event、传递给 `data` 属性的数组中该节点所对应的对象、节点对应的 Node、节点组件本身。 |
| check-change     | 当复选框被点击的时候触发               | 共三个参数，依次为：传递给 data 属性的数组中该节点所对应的对象、节点本身是否被选中、节点的子树中是否有被选中的节点 |
| check           | 点击节点复选框之后触发                 | 共两个参数，依次为：传递给 data 属性的数组中该节点所对应的对象、树目前的选中状态对象，包含 checkedNodes、checkedKeys、halfCheckedNodes、halfCheckedKeys 四个属性 |

## 方法

| 方法名          | 说明                                                | 参数                                                         |
| --------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| setCheckedNodes | 设置目前勾选的节点                                  | 要选中的节点构成的数组                                       |
| setChecked      | 设置节点是否被选中                                  | (key/data, checked, deep) 接收三个参数: 1. 要选中的节点的 key 或者数据 2. 一个布尔类型参数表明是否选中. 3. 一个布尔类型参数表明是否递归选中子节点 |
| setCheckedKeys  | 设置目前选中的节点                                  | (keys, leafOnly) 接收两个参数: 1. 一个需要被选中的多节点 key 的数组 2. 一个布尔类型参数，默认为 `false`. 如果参数是 `true`, 它只返回当前选择的子节点数组。 |
| getCheckedKeys  | 若节点可用被选中，它将返回当前选中节点 key 的数组   | (leafOnly) 接收一个布尔类型参数，默认为 `false`. 如果参数是 `true`, 它只返回当前选择的子节点数组。 |
| getCheckedNodes | 如果节点可以被选中， 本方法将返回当前选中节点的数组 | (leafOnly, includeHalfChecked) 接收两个布尔类型参数: 1. 默认值为 `false`. 若参数为 `true`, 它将返回当前选中节点的子节点 2. 默认值为 `false`. 如果参数为 `true`, 返回值包含半选中节点数据 |
| getCurrentKey                |         返回当前被选中节点的数据 (如果没有则返回 null)                                           | —— |
| getCurrentNode                |        返回当前被选中节点的数据 (如果没有则返回 null)                                            | —— |
| setCurrentKey                |         通过 key 设置某个节点的当前选中状态，使用此方法必须设置 node-key  属性                                           | (key, shouldAutoExpandParent=true) 1. 待被选节点的 key， 如果为 `null`, 取消当前选中的节点 2. 是否自动展开父节点 |
| setCurrentNode                |        设置节点为选中状态，使用此方法必须设置 node-key 属性                                            | (node, shouldAutoExpandParent=true) 1. 待被选中的节点 2. 是否展开父节点 |

插槽

| 插槽名      | 说明                                   | 参数 |
| ----------- | -------------------------------------- | ---- |
| tree-search | 自定义搜索（配合showSearch为true使用） | ———— |

