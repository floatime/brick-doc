#  sso-common 组件

> 前言
>
> `brick-common`依赖了Sa-Token
> 




## 引入依赖

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sso-common</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>
```



## 注解鉴权
> 所谓注解鉴权，就是通过注解的方式去判断一个账号是否拥有指定权限，有就通过，没有就禁止访问，优雅的将鉴权与业务代码分离。
>
> 不过使用拦截器模式，只能把注解写在Controller层

### @SaCheckLogin
> 登录校验 —— 只有登录之后才能进入该方法。
> 
> 方法上加该注解代表该方法需登录之后才能访问，类上加该注解代表该类下的所有方法都需要登录之后才能访问

?> 测试数据

```javascript
@SaCheckLogin
@RequestMapping("info")
public int test(){
  return 10;
}
//如未登录，会返回结果{"code":"SERVER_NO_LOGIN","data":null,"message":"授权中心未登录","status":101,"success":false,"timestamp":1681183703781,"txId":""}
```
### @SaIgnore
> 忽略校验 —— 表示被修饰的方法或类无需进行注解鉴权和路由拦截器鉴权，以游客身份访问。
> 
> @SaIgnore 修饰方法时代表这个方法可以被游客访问，修饰类时代表这个类中的所有接口都可以游客访问。
> 
> @SaIgnore 具有最高优先级，当 @SaIgnore 和其它鉴权注解一起出现时，其它鉴权注解都将被忽略。
> 
> @SaIgnore 同样可以忽略掉 Sa-Token 拦截器中的路由鉴权
> 

?> 测试数据

```javascript
@SaIgnore
@RequestMapping("info")
public int test(){
  return 10;
}
//返回结果{"code":"SUCCESS","data":10,"message":"请求成功","status":200,"success":true,"timestamp":1681183795164,"txId":""}
```
### 模拟设置当前账号权限码、角色标识集合
```javascript
/**
 * 自定义权限验证接口扩展
 */
@Component    // 保证此类被SpringBoot扫描，完成Sa-Token的自定义权限验证扩展 
public class StpInterfaceImpl implements StpInterface {
    //模拟设置账号的权限码集合
    @Override
    public List<String> getPermissionList(Object o, String s) {
        List<String> result = new ArrayList<>();
        result.add("a.*");
        result.add("user.add");
        result.add("101");
        return result;
    }
    //模拟设置账号的角色标识集合
    @Override
    public List<String> getRoleList(Object o, String s) {
        List<String> result = new ArrayList<>();
        result.add("admin1");
        result.add("admin2");
        result.add("admin3");
        return result;
    }
}
```
### @SaCheckRole
> 角色校验 —— 必须具有指定角色标识才能进入该方法。
>
?> 测试数据

```javascript
// 权限校验：必须具有指定权限才能进入该方法 
    @SaCheckRole("admin1")      //true 
    @RequestMapping("add")
    public String add() {
            return "用户增加";
    }

    @SaCheckRole(value = {"admin","admin1"})      //false mode默认为SaMode.AND 需同时满足admin admin1
    @RequestMapping("add")
    public String add() {
           return "用户增加";
    }

    @SaCheckRole(value = {"admin","admin1"},mode = SaMode.OR)      //true 满足其中一个即可
    @RequestMapping("add")
    public String add() {
           return "用户增加";
    }
```

### @SaCheckPermission
> 权限或角色校验，具有指定权限或指定角色即可通过校验
> 

?> 测试数据

```javascript
    //单个权限校验
    @GetMapping("/list")
    @SaCheckPermission("user.add")
    public String list() {
        return "inspect/pro-pac-classify/index";
    }
    
    //多个权限校验，必须同时满足
    @GetMapping("/list")
    @SaCheckPermission(value = {"user.add", "a.add", "102"}, mode = SaMode.AND)
    public String list() {
        return "inspect/pro-pac-classify/index";
    }
    返回结果 无此权限：102
    
    //多个权限校验，满足其中一个即可
    @GetMapping("/list")
    @SaCheckPermission(value = {"user.add", "a.add", "102"}, mode = SaMode.OR)
    public String list() {
        return "inspect/pro-pac-classify/index";
    }
    
    //mode介绍
    mode有两种取值(专门配合权限校验使用)：
    SaMode.AND, 标注一组权限，会话必须全部具有才可通过校验。(默认)
    SaMode.OR, 标注一组权限，会话只要具有其一即可通过校验。

    //角色权限双重“or校验” 具备指定权限或者指定角色即可通过校验
    @GetMapping("/list")
    @SaCheckPermission(value = "user.del",orRole = "admin1")
    public String list() {
        return "inspect/pro-pac-classify/index";
    }

    //orRole介绍
    orRole 字段代表权限认证未通过时的次要选择，两者只要其一认证成功即可通过校验，其有三种写法：
     写法一：orRole = "admin"，代表需要拥有角色 admin 。
     写法二：orRole = {"admin", "manager", "staff"}，代表具有三个角色其一即可。
     写法三：orRole = {"admin, manager, staff"}，代表必须同时具有三个角色。
```
### 权限通配符
>允许你根据通配符指定泛权限，例如当一个账号拥有art.*的权限时，art.add、art.delete、art.update都将匹配通过
>
```javascript
// 当拥有 art.* 权限时
StpUtil.hasPermission("art.add");        // true
StpUtil.hasPermission("art.update");     // true
StpUtil.hasPermission("goods.add");      // false

// 当拥有 *.delete 权限时
StpUtil.hasPermission("art.delete");      // true
StpUtil.hasPermission("user.delete");     // true
StpUtil.hasPermission("user.update");     // false

// 当拥有 *.js 权限时
StpUtil.hasPermission("index.js");        // true
StpUtil.hasPermission("index.css");       // false
StpUtil.hasPermission("index.html");      // false
```
### 判断下面哪些方法通过校验
```javascript
    @GetMapping("/list")
    @SaCheckPermission(value = {"user.del","user.add","user.upd"},orRole = {"admin1,admin2,admin5"})
    public String list() {
        return "inspect/pro-pac-classify/index";
    }
    //false 既不满足权限校验，也不满足角色校验

    @GetMapping("/list")
    @SaCheckPermission(value = {"user.del","user.add","user.upd"},orRole = {"admin1“,”admin2“,”admin5"})
    public String list() {
         return "inspect/pro-pac-classify/index";
    }
    //true 满足了角色校验

    @GetMapping("/list")
    @SaCheckPermission(value = {"user.del","user.add","user.upd"},mode =SaMode.OR ,orRole = {"admin1,admin2,admin5"})
    public String list() {
         return "inspect/pro-pac-classify/index";
    }
    //true 满足了权限校验
```