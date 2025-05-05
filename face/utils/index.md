# 工具

# 网络请求

>地址
>/utils/request.js

>引用
>import request from '/utils/request.js'

```js
    import request from "../utils/request.js";

    let requestConfig1={
        baseUrl:'https://www.baidu1.com'
    }
    let requestConfig2={
        baseUrl:'https://www.baidu2.com'
    }
    //传入配置文件指定访问域名
    //多域名使用
    const requestServe1 =  request(requestConfig1)
    const requestServe2 =  request(requestConfig2)

    //域名被初始化
    requestServe1({
        url: '/aaa',
        method: 'post',
    }).then(()=>{
        
    })
    requestServe2({
        url: '/bbb',
        method: 'post',
    }).then(()=>{
        
    })
	
	//域名没有初始化 或者不使用被初始化的域名可以传入参数baseUrl	
	request()({
        baseUrl:'https://www.baidu3.com'
	    url: '/CCC',
	    method: 'post',
	}).then(()=>{
	    
	})
```
创建一个异步的网络请求,返回一个Promise对象

resolve 参数为 api 返回值

该方法做了对错误的处理，如果status不为200，会被抛出 reject，正常则返回 resolve

#### 请求方式

> post  ||  put

```js
//请求需要传入的参数
let data={
    name:'名字',
    age:'年龄',
}

request()({
    url: '/aaa',
    method: 'post',
    data : data
}).then(()=>{
    
})
```

> get

```js
//请求需要传入的参数
let data={
    name:'名字',
    age:'年龄',
}
request()({
    url: '/aaa',
    method: 'get',
    params : data
}).then(()=>{
    
})

//请求路径直接拼接参数
let id=10001
request()({
    url: '/aaa/'+id,
    method: 'get',
}).then(()=>{
    
})
```

# 提交数据并跳转页面

该方法使用后会跳转至返回的页面

> 地址
> /utils/formTransmit.js

> 引用
> import { formRoute } from '/utils/formTransmit.js'

> 使用

```js
//要提交的数据
let data = {
    name: '名字',
    age: 17,
    address:['北京市','北京市','东城区']
}
let url = 'https://baidu.com'
let method = 'get'
/**
 * @param data 要传递的数据
 * @param url  访问api地址
 * @param method 请求类型 默认为post
 */
formRoute(data, url, method)
```