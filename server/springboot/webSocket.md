# WebSocket 组件

> 前言
>
> 该组件依赖于SpringBoot环境
>
> 使用WebSocket组件,以实现站内信或及时通讯功能

## 引入依赖

```xml

<dependency>
    <groupId>cn.ifloat.brick.sprofile</groupId>
    <artifactId>brick-sprofile-websocket</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

## 配置信息

```yml
brick:
  websocket:
    config:
      isCluster: false #是否开启集群模式
      redisKeyGroup: default
      isLongHold: false #是否保持长链接
      sessionTimeout: 300000 #会话时长(毫秒)
```

## 使用

### 服务端

> 实现 **WebSocketEvent** ,交由**IOC**管理,并指定**name**

```java

@Component
public class BStationPremiseEvent implements WebSocketEvent {

    @Autowired
    private SendMessageKit sendMessageKit;

    @Override
    public String name() {
        //指定name
        return "name";
    }

    @Override
    public void init() {
        //初始化
        logger.info("example station websocket event init");
    }

    @Override
    public void open(String s, String s1) {
        //前端打开链接时触发
        logger.info("example station websocket event open:{},{}", s, s1);
    }

    @Override
    public void message(WsMessage wsMessage) {
        //前端发送消息时触发
        logger.info("example station websocket event message:{}", wsMessage.toString());
        sendMessageKit.send(wsMessage);
    }

    @Override
    public void error(Throwable throwable, String s) {
        //发送消息,出现错误时触发
        logger.info("{}example station websocket event error:{}", s, throwable.toString());
    }

    @Override
    public void close(String s) {
        //链接关闭时触发
        logger.info("example station websocket event close:{}", s);
    }
}
```

### SendMessageKit

> SendMessageKit
>
> 中包含了发送通知及创建分组等方法

```java
@Autowired
private SendMessageKit sendMessageKit;
```

|                       方法名                        |        描述        |           入参            |                返回参数                |
| :-------------------------------------------------: | :----------------: | :-----------------------: | :------------------------------------: |
|               send(WsMessage message)               |      发送通知      |       WsMessage对象       |                   无                   |
| registerGroup(String group, Collection<String> ids) |      创建分组      |  分组名称,分组成员codes   |                   无                   |
|         addNodeFor(String group, String id)         |    分组添加成员    | 分组名称,新增分组成员code |                   无                   |
|             nodesByGroup(String group)              | 获取分组内全部成员 |         分组名称          | Collection<String> <br />分组成员codes |

### WsMessage

> **前端**发送通知,以及通过**SendMessageKit**发送通知的入参必须为**WsMessage** 对象

```java
//WsMessage对象
public class WsMessage {
    private String send;//发送人CODE
    private String receive;//接收人CODE
    private String message;//信息内容
    private Date sendTime = new Date();//发送事件
    private String type;//通知类型
    private int receiveType;//接收人类型
}
```

> 接收人类型
>
> 枚举类ReceiveType
>
> 当接收人类型为 **点发** 时,接收人CODE为对应的**接收人**CODE
>
> 当接收人类型为 **组发** 时,接收人CODE为对应的**分组**CODE

| 类型 | 对应值 | 描述 |
| :---: | :----: | :--: |
| POINT | 0 | 点发 |
| GROUP | -1 | 组发 |
| HEART | -3 | 心跳 |

### 客户端

> 创建WebSocket链接
>
> 链接中的**name**与实现了**WebSocketEvent**中指定的**name**一致,
>
> 发送消息时,通过name来匹配对应的Event,Event可以创建多个

```js
var ws = new WebSocket(ws
://localhost:"+port+"/brick/ws/name/"+id);
```

> WebSocket 事件

|  事件   |     事件方法     |            描述            |
| :-----: | :--------------: | :------------------------: |
|  open   |  Socket.onopen   |       连接建立时触发       |
| message | Socket.onmessage | 客户端接收服务端数据时触发 |
|  error  |  Socket.onerror  |     通信发生错误时触发     |
|  close  |  Socket.onclose  |       连接关闭时触发       |

> WebSocket 方法

|       方法       |       描述       |
| :--------------: | :--------------: |
| Socket.send(msg) | 使用链接发送数据 |
|  Socket.close()  |     关闭链接     |

> 示例

```html

<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>WebSocket</title>
    </head>
    <body>
        <h1>WebSocket 示例</h1>
        <input id="con" name="con" value/>
        <input id="port" name="port" value/>
        <button onclick="connection()">连接服务</button>
        <p/>
        <input id="wsmsg" name="wsmsg" value='{"send":"","receive":"","message":"","type":""}'/>
        <button onclick="submitWs()">Submit</button>
        <button onclick="closedWs()">Closed</button>
    </body>
    <script>
        function getUrl(port, id) {
            return "ws://localhost:" + port + "/brick/ws/station/" + id;
        }

        var ws = null;

        function connection() {
            var name = document.getElementById("con").value;
            var port = document.getElementById("port").value;
            ws = new WebSocket(getUrl(name, port));
            ws.onopen = function () {
                alert("started")
            }
            ws.onmessage = function (e) {
                alert("onmessage:" + e.data);
            }
            ws.onclose = function () {
                alert("closed")
            }
        }

        function submitWs() {
            console.log("submitWs method");
            var msg = document.getElementById("wsmsg").value;
            ws.send(msg);
        }

        function closedWs() {
            console.log("closedWs method");
            ws.close();
        }

    </script>
</html>
```
