# Httpclients 组件

>前言
>
>brick-httpclients 封装了 `Apache httpclients` 增加统一且灵活配置,增加`全局日志拦截器` `重试拦截器` `适配Spring环境`
>
>[依赖 brick-httpclients ](/getter?id=brick-httpclients)
>
>在`SpringBoot` 环境下使用`Httpclients`组件时参考 参考 [SpringBoot 环境下的组件]() 

?> 前期准备, 我们需要一个最简单且最实用的配置 用来定义httpclient 的默认行为 详细配置请参考: [httpclients详细配置]()

```javascript
//操作工具类
HttpBasicClient client = new HttpBasicClient();
//设置全局日志处理器
client.reSettingGolbalLog(
  new HttpLog() {
  		@Override
  		public void log(LogDesc logDesc) {
 				 System.out.println("自定义http 日志: "+logDesc.getParameter()+
                            " "+logDesc.getResult()+" "+logDesc.getStatus());
            }
        });

```

## 常用操作

```javascript
// GET方式请求
BasicHttpResult result = client.getReq("https://baidu.com");
result.getContent();

// POST方式请求  构建表单参数
FormItem.mapToArray(map); //map转form表单
FormItem item1 = FormItem.createFileItem("file", file); //构建file 表单
FormItem item2 = FormItem.createTextItem("text", value);//构建普通表单
BasicHttpResult result=client.postBasicFormBody("url", new FormItem[]{item1, item2}); //POST 方式请求
result.getContent(); //http响应结果
result.getStatus();  //http响应状态码

// POST方式请求 json请求体
BasicHttpResult result = client.postJsonBody("url",jsonStr);

// POST方式请求 xml请求体
BasicHttpResult result = client.postXmlBody("url",xmlStr);

// POST方式请求 map参数
client.postFormBodyByMap("url",map);


```

## 流式风格调用

?> brick-httpclients 也提供了流式调用支持全部请求方式 且都 适配全局参数设置

```javascript
// GET 方式调用 
BasicHttpResult result = client.
                fluentClientBuilder().
                httpGet("url").
                header("hk", "hv"). //指定header 可以设置多个
                gizpDecompressing(). //设置请求体压缩
                complexResult(). // 设置返回最简单的结果
                builder(). //构建
                request(); //请求,并打印全局日志

//POST 方式调用
result = client.fluentClientBuilder().
    httpPost("url").
    jsonEntity("jsonStr").
    gizpDecompressing().
    complexResult().
    builder().
    request();

// DELETE 方式调用
result = client.fluentClientBuilder().
    httpDelete("url").
    gizpDecompressing().
    complexResult().
    builder().
    request();

// PUT 方式调用
result = client.fluentClientBuilder().
    httpPut("url").
    jsonEntity("jsonStr").
    gizpDecompressing().
    complexResult().
    builder().
    request();

// OPTIONS 方式调用
result = client.fluentClientBuilder().
    httpOptions("url").
    gizpDecompressing().
    complexResult().
    builder().
    request();
```

## 运行时全局设置

!> client 可以在运行时改变部分全局设置包括`requestConfig headers httpContext httpConfig`

```javascript
// 将所有需要修改的配置在对应的 方法中修改后并返回
RuntimeSettingsDefaultSection section=new RuntimeSettingsDefaultSection() {
  @Override
  public HttpConfig reSettingConfig(HttpConfig config) {
    return config;
  }

  @Override
  public RequestConfig reSettingReuqestConfig(RequestConfig requestConfig) {
    return requestConfig;
  }

  @Override
  public Header[] reSettingGolbalHeaders() {
    return new Header[0];
  }

  @Override
  public HttpClientContext reSettingGolbalHttpContext() {
    return null;
  }
};
//调用不同的重置方法,运行时读取最新的全局配置
client.reSettingConfig(section);
client.reSettingGolbalHeader(section);
client.reSettingGolbalLog(null);
client.reSettingGolbalHttpContext(section);
client.reSettingGolbalRequestConfig(section);
```



