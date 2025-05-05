#  Resubmit防止表单重复提交 组件

```javascript
前言
平时开发的项目中可能会出现下面这些情况：
   1.由于用户误操作，多次点击表单提交按钮。
   2.由于网速等原因造成页面卡顿，用户重复刷新提交页面。
   3.恶意用户使用postman等工具重复恶意提交表单（攻击网站）。
这些情况都会导致表单重复提交，造成数据重复，增加服务器负载，严重甚至会造成服务器宕机。因此有效防止表单重复提交有一定的必要性。
需注意通过js代码，当用户点击提交按钮后，屏蔽提交按钮使用户无法点击提交按钮或点击无效，从而实现防止表单重复提交这种方法js代码很容易被绕过。
比如用户通过刷新页面方式，或使用postman等工具绕过前端页面仍能重复提交表单，因此不推荐单用此方法，使用token令牌来防止表单重复提交。
```


## 引入依赖

```xml
<dependency>
    <groupId>cn.ifloat.brick.sprofile</groupId>
    <artifactId>brick-sprofile-cache</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```



## 配置信息

```yml
brick:
  resubmit:
    config:
      type: REDIS            #缓存类型（本地为LOCAL）
      keyGroup: insp-server  #分组名（nacos里配置的redis客户端分组名）
      limitCount: 200        #限制数量（只对type为LOCAL本地的数量做限制）       
      expireTime: 120       #失效时间 单位秒
      enabled: true          #是否开启防止重复提交 ture开启 false不开启 默认为true
```
## 防止表单重复提交使用

> 通过注解 @BRepeatSubmit 
>
> BRepeatSubmit文件里可设置请求头参数来接收前端传的token值，默认为repeat_submit_token
>
>

## eg：举列说明
```javascript
//先验证token是否为空，如果为空直接返回token丢失，如果不为空，把token当作key去缓存中查询是否存在，不存在添加进缓存中中，然后执行方法。如果存在，直接返回请不要重复提交
@BRepeatSubmit
public Boolean addUser(User user) {
  return userService.addUser(user);
}
```
