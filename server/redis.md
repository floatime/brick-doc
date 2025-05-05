# redis 组件

> 前言
>
> brick-redis 封装了 redisson 对redis 的大部分操作, 为统一拦截redission 行为增加`业务前缀`  `业务锁前缀` `避免僵尸key ,简化使用过程
>
> [依赖 brick-redis ](/getter?brick-redis) 
>
> 在`SpringBoot` 环境下使用redis 组件时参考 参考 [SpringBoot 环境下的组件]() 

?> 前期准备 我们先提供一个最简单的示例,初始化redis客户端之前需要提供 [redis详细配置]()

``` javascript
//客户端相关配置
RedissonConfig config = new RedissonConfig();
//指定key的分组 就是所有可以的前缀
config.setKeyGroup("float");
//设置是单节点还是多节点
config.setIsSingle(true);
//设置对应的redis ip:port
config.setNodeAddress("127.0.0.1:6379");
//core 就是创建所有操作工具类的入口
RedissonCore redisCore = new RedissonCore(config);
```

## String 普通操作

!> 在redis中key为 float@key 依据 keyGroup参数而定 ,所有没有指定失效时间的 存入api 则默认失效时间是一天 

```javascript
//普通操作 也是最常用的工具类
BasicDataStruct<String> strOps = redisCore.strOps();

//向redis中存入
strOps.set("key","value"); 
strOps.set("key","val",10, TimeUnit.SECONDS); //指定过期时间 10秒
RFuture<Void> future = strOps.setAsync("key", "val"); //异步执行直到future.get 阻塞返回
boolean flag = strOps.setnx("key","val"); //同 reidis sennx
strOps.setnxAsync("key","val"); //异步操作
RFuture<Boolean> future = strOps.setnxAsync("key","val",10,TimeUnit.HOURS); //异步执行指定过期时间 10小时
String val = strOps.getSet("key","val"); //同 redis getset
RFuture<String> future = strOps.getSetAsync("key", "val"); //异步执行
RFuture<String> future2 = strOps.getSetAsync("key", "val",10,TimeUnit.DAYS); //指定失效时间 10天

//获取缓存
String value = strOps.get("key"); //直接获取
String value = strOps.getDel("key"); //删除并返回删除前的值

//过期时间
strOps.expireBySeconds("key",1);//以秒为单位
strOps.expireByHours("key",1);//以小时为单位
strOps.expireByMinutes("key",1);//以分钟为单位
strOps.clearExpire("key"); //清除过期时间

//其他操作
strOps.size("key"); //返回value的长度
strOps.delete("A","B","C"); //删除多个key
Long times = strOps.ttl("key"); //查询某个key的过期时间

```

## Map 操作

!> 同String 操作一样 key会受到 keyGroup作为前缀拼接, 且存入的命令没有指定过期时间 则默认为一天

```javascript
// map 数据结构工具类
MapDataStruct<String, String> mapOps = redisCore.mapOps();
// 插入
mapOps.put("mk","itemKey1","itemVal1");
mapOps.putAll("mk", new HashMap<>()); //批量插入
boolean flag = mapOps.fastPut("mk", "itemKey1", "itemVal2"); //检查是否存在

//获取
Collection<String> values = mapOps.values("mk"); //获取所有值
Set<String> keys = mapOps.keySet("mk"); //获取所有key
mapOps.readAllMap("mk"); //获取map

mapOps.remove("mk", "itemKey1"); //删除map中的某个key
mapOps.delete("mk"); //删除mk对应的所有键值对

```

## Set 操作

!> 同String操作一样会受到keyGroup走位前缀拼接,且存入的命令没有指定过期时间,则默认为一天

```javascript
// set数据结构工具类
SetDataStruct<String> setOps = redisCore.setOps();
// 插入
setOps.sadd("sk", "sv");
setOps.saddAll("sk", new HashSet<>());
//检查
setOps.isEmpty("sk");
//清除
setOps.clear("sk");
//获取
setOps.toArray("sk");
       
```

## List 操作

!> 同String操作一样会受到keyGroup走位前缀拼接,且存入的命令没有指定过期时间,则默认为一天

```javascript
//list数据结构工具类
ListDataStruct<String> listOps = redisCore.listOps();
//插入
listOps.add("lk", "lv");
listOps.addAll("lk", new ArrayList<>());
//清除
listOps.clear("lk");
//获取
listOps.get("lk");
//返回list.size
listOps.size("lk"); 
//指定在某个元素之前还是之后添加
listOps.addAfter("lk","lv","lv2");
listOps.addBefore("lk", "lv", "lv2");
```

## 复合类型操作

!> 所有的 操作工具类都支持对值的 序列化反序列化, 可以将对象本身存到redis中`(但是性能要考虑,毕竟序列化反序列化会消耗性能)`

```javascript
//目前只支持 Json序列化的方式用下列示例可以对value进行存储 在插入时回将值序列化JSON存入redis ,获取会反序列化成Object
SetDataStruct setOps = redisCore.setOps(Codec.Json.value());
ListDataStruct listOps = redisCore.listOps(Codec.Json.value());
MapDataStruct mapOps = redisCore.mapOps(Codec.Json.value());
BasicDataStruct pojoOps = redisCore.pojoOps(Codec.Json.value());
```

## 分布式锁

!> 锁的key 也是跟keyGroup相关联, 分布式锁在高可用场景下 经常用到,用来解决不同进程下 强多同一数据资源产生的数据安全问题

> 首先redisson 支持 `可重入锁(Lock)` `公平锁(FairLock)` `联锁(MultiLock)` `红锁(RedLock)` 

```javascript
//获取锁的工具类
RedisLock lockOps = redisCore.lockOps();
//获取重入锁 *
lockOps.getLock("lockKey");
//获取红锁
lockOps.getRedLock(lock1,lock2,lockN);
//获取公平锁 *
lockOps.getFairLock("lockKey");
//获取联锁
lockOps.getMultiLock(lock1,lock2,lockN);

//使用锁的场景 1
//尝试着获取锁
RLock lock = lockOps.getFairLock("lock");
//加锁,如果为争取到锁 会阻塞住
lock.lock(); 

//业务逻辑.....

//处理完业务逻辑释放锁
lock.unlock();


//使用场景 2
RLock lock = lockOps.getFairLock("lock");
//加锁,如果为争取到锁 会阻塞住
boolean flag = lock.tryLock(); 

if(!flag){
  throw new RuntimException("当前业务的数据被占用,请稍后再试");
}
//业务逻辑.....

//处理完业务逻辑释放锁
lock.unlock();
```

> 两种使用场景的区别在于阻塞住等待资源释放之后重入去争抢锁资源 和 发现锁被占用就释放掉,转换成业务异常抛出
>
> 这有点类似于 小明在排队等着上厕所,有两种选择 是在门口继续等待,还是不等了过会再来 是一样的,看具体业务需求
