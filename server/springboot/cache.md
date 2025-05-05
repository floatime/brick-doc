#  Cache缓存 组件

> 前言
>
> 缓存就是数据交换的缓冲区（称作：Cache）
> 
> 使用场景：如果发现有大量数据需要频繁查询使用，或者某些数据不会频繁变更时，为了提高数据库IO性能（避免频繁连接数据库），可以使用缓存，
> 每次查询先查缓存，缓存没有查到再去数据库查询，然后存入缓存，提高读取效率。
>



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
  cache:
    config:
      type: REDIS            #缓存类型（本地为LOCAL）
      keyGroup: insp-server  #分组名（nacos里配置的redis客户端分组名）
      limitCount: 200        #限制数量（只对type为LOCAL本地的数量做限制）       
      expireTime: 7200       #失效时间 单位秒
      enabled: true          #是否开启缓存 ture开启 false不开启 默认为true
```
## 缓存使用

> 通过注解 @BCacheable(group = "EXTRAS_", key = "${args[0].id}", putOrDelete = true)  
>
> group为key的分组，也就是key的前缀，主要为了区分key。假设一个用户id为1做为key的用户信息加入缓存中，此时一个订单id为1做为key的订单信息也
> 要加入缓存中，就会替换key为1对应的value值，这时需从缓存中获取用户id为1的患者信息，获取到的却是订单信息，所以需要给key做分组进行区分。
> 
> key为存储到缓存中的键，我们需要做成动态key，使用表达式获取内置对象属性值来当作key，如果写死值的话难免会出现多人开发一个项目死值重复出现。
> - 目前内置对象有两个属性：
>  - args 获取入参中某个参数 假设获取入参中第一个参数(id)做为key：${args[0]} 假设获取入参中第一个参数(user)中id属性做为key：${args[0].id} 
>  - current 获取当前登陆人信息 假设用当前登陆人的id做为key：${current.id}
> 
> putOrDelete判断是种缓存还是删缓存 true为种缓存 false为删缓存 默认为true
>

## eg：举列说明
```javascript
//查询 种缓存 先去redis中查询是否有key为COO_EXTRAS_${args[0]}对象 如果有直接返回，如果没有去查库，获取数据存入缓存
@BCacheable(group = "COO_EXTRAS_", key = "${args[0]}", putOrDelete = true)
public User getUser(Long userId) {
  return userService.getInfo(userId);
}
//修改 删缓存 先更新数据库中数据信息 再通过key去redis中删除对应缓存
@BCacheable(group = "COO_EXTRAS_", key = "${args[0].userId}", putOrDelete = false)
public Boolean updUser(User user) {
  return userService.updUser(user);
}
```
## ps：删除缓存

- 先删除缓存，再更新数据库。
  - 这个操作有一个比较大的问题，在对缓存删除完之后，有一个读请求，这个时候由于缓存被删除所以直接会读库，读操作的数据是老的并且会被加载进入缓存当中，后续读请求全部访问的老数据。
- 先更新数据库，再删除缓存（推荐）
  - 有一个数据此时是没有缓存的，所以查询请求会直接落库，更新操作在查询请求之后，但是更新操作删除数据库操作在查询完之后回填缓存之前，就会导致我们缓存中和数据库出现缓存不一致，但是这个问题几率特别小。
- 为什么要删除缓存，而不是更新缓存
  - 你可以想想当有多个并发的请求更新数据，你并不能保证更新数据库的顺序和更新缓存的顺序一致，那就会出现数据库中和缓存中数据不一致的情况。所以一般来说考虑删除缓存。

# 