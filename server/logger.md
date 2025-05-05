# Logger 组件

> 前言
>
> `brick-logger` 封装Logback做为核心工具, 其中定义了特定的日志级别,和产生的日志文件规范
>
> 它和其他的组件有一点区别(日志本身就伴随着整个开发周期),部分功能只在Spring环境下起作用,因为它一来了Aop切面的概念  
>
> 它依赖 [brick-logger组件](/getter?id=brick-logger)

## 常用操作

```javascript
// system() 代表 SYSTEM loggerName 的日志会输出到对应的日志文件中
LogKit.system().info("当前登录人:{} 登录时间:{} 在位置:{} 操作:{}",name,date,location,action);
// operation() 代表 OPERATION loggerName 的日志会输出到对应的日志文件中
LogKit.operation().info("这是一条简单的日志测试");
//logger 一般声明在类作为静态全局成员变量 loggerName位全类名
final Logger logger = LoggerFactory.getLogger(this.getClass());
//每个不同的logger 对应的 日志等级 要了解日志等级可以参考
logger.debug("");
logger.info("");
logger.warn("");
//error级别的日志可以接收一个 exception 作为日志输出
try{
  int i = 1 / 0;
}catch(Exception e){
  LogKit.system().error("可以包含 exception的日志",e);
}
```



## 日志输出

>日志是根据日志名称区分可以单独设置不同的`loggerName`输出到不同的配置文件,在项目中约定常用的`loggerName`
>
>SYSTEM: 输出到 system.log 下
>
>OPERATION:输出到 operation.log 下
>
>其他loggerName的日志默认在控制台上输出
>
>所有loggerName 都可以用日志级别控制是否满足输出需求
>
>

## Spring环境下的日志处理

!> 当前日志操作,依赖Spring 的IOC 和AOP 功能需要再Spring环境下使用

?> @PerformanceLog(unit = PerformanceUnit.MILLS) 记录函数的性能时间记录

```javascript
@RequestMapping("list")
//执行函数后记录开始和结束时间自动计算总共消耗的时间,灵活的单位定义,默认debug级别
@PerformanceLog(unit = PerformanceUnit.MILLS) // NS:纳秒,MILLS:毫秒,SECONDS:秒,MINUTES:分钟,HOUR:小时,DAY:天
public String list(Model model) {return "list"}
```

?> @OperationLog 记录操作日志支持日志模版定义,支持参数注入

```javascript
@OperationLog(
  //日志模版,使用内置参数
  logTlt = "操作日志测试 ${result} : ${now} ", 
  //定义操作日志的应用 方便以后根据业务应用区分日志
  application = "example", 
  //定义操作日志的业务,和application组成了两个维度
  service = "user", 
  //条件表达式,方便以后定位数据范围
  condition = "")
public String list(Model model) {return "list"}
```

## 日志启动参数

```javascript
//指定日志 contextName,用来在日志分析区分属于哪个日志空间
brick.logger.cn:输出日志时指定项目名称
//所有产生的日志文件输出到哪个目录 默认: ${usr.dir}
brick.logger.root.path:输出目录的根目录"以/结尾"
//设置文件大小限制,超过部分做日志分割成多个文件
brick.logger.maxfilesize:最大文件容量大小 例：256M
//设置文件最大保存数量 多余的部分自动清除
brick.logger.maxhistory:最大保存文件数量个数
//设置归档日志文件的总容量
brick.logger.totalsizecap:保存归档文件的总容量

//设置loggerName位父节点的日志级别
brick.logger.root.level root日志级别
//设置操作日志的界别 默认 info
brick.logger.operation.level 操作日志级别
//设置系统日志的输出级别 默认info
brick.logger.system.level  系统日志级别
//设置性能日志的日志级别 默认 debugg
brick.logger.performance.level  性能日志级别
// 设置单个应用的日志级别
brick.logger.app.level  应用日志级别
// 为当前项目设置一个loggerName 父节点
brick.logger.app.root   应用root扫描的包

//java -jar 时注入日志参数
java -jar -Dbrick.logger.root.leve=DEBUG -Dbrick.logger.app.root=cn.demo.a demo.jar 
```

