# paging组件


>地址
>/components/paging/bigPaging.js

>引用
>import bigPaging from '/components/paging/bigPaging.js'

>注册
>components 对象内注册 bigPaging 和引用时起的名字相同

>使用

```js
// html
<big-paging  :config="config"></big-paging>
// js
data(){
    return{
        config: {
    	    pageSize: 10,
    	    pageNum: 1,
    	    total: 10,
         }
    }
     
}
```
![button-type](../../assets/image/face/page-style/paging.png)

## 属性
| 参数名   | 数据类型 | 说明         | 默认值 |
| -------- | -------- | ------------ | ------ |
| config   | Object   | 基本数据     | 必填   |
| render   | Object   | 控制渲染样式 | {}     |
| event    | Object   | 方法         | {}     |
| behavior | Object   | 行为         | {}     |

```js
config: {
    // 每页显示条目个数
	pageSize: 10,
    // 当前页数
	pageNum: 1,
    // 总条数
	total: 10,
}
```

## 事件

| 事件名      | 说明                                                     | 参数 |
| ----------- | -------------------------------------------------------- | ---- |
| change-page | 当前分页发生改变时触发(刷新,页条目数改变,当前页发生改变) | --   |
|             |                                                          |      |
|             |                                                          |      |

