# Common 组件

> 前言 
>
> `brick-common` 包含常用的最基本的操作.和一些第三方依赖比如 `beetl`	 `FastJson`	 Apach commons 套件`` 等.
>
> 为能统一代码风格和规范, 避免不必要的资源浪费, 提高阅读效率,下面介绍常用的基本操作`(无需关心性能消耗,实现逻辑中尽量考虑到缓存加载的问题避免内存过度使用)`
>
> [依赖 brick-common 组件](/getter?id=brick-basic-common)

## Bean 常用操作

!> 前期需要一些公共的数据实例,以便更清晰的说明工具的含义

```javascript
//基类
public class BaseBean implements Serializable {
    private Long id;
  	//getter setter constructor
}
//ApiType 集成于 BaseBean
public class ApiType extends BaseBean {
	private String typeName;
	private String typeCode;
	private Long typeParentId;
	private Integer operationUser;
	private java.util.Date updateTime;
	private String realUrl;
	private String viewFlag;
  private Long referenceId;
	// getter setter constructor toString()
}
//ApiTypeView 虽然和ApiType 名字看起来很像,但没有任何继承关系 只是部份属性声明相同,这样更突出工具的行为
public class ApiTypeView{
  private String typeName;
  private String typeCode;
  private Long typeParentId;
	// getter setter constructor toString()
}
```

### BeanUtils 常用操作

?> `cn.ifloat.brick.basic.common.toolkit.beans.BeanUtils`

?> 公共数据示例,给组件提供测试数据

```javascript
//测试数据容器
List<ApiType> list = new ArrayList<>();
//在所有test example 之前执行,为 list 造测试数据
@Before
public void before(){
  for (int i = 0; i < 10; i++) {
    ApiType apiType = new ApiType(
      "typeName-" + i, "typeCode-" + i, (long) i, 
      10, new Date(), "url-"+i, "viewFlag-" + i, 100l+i);
    list.add(apiType);
  }
}
//在所有test example 之后执行
@After
public void after(){
  list = null;
}
```

?> 拷贝集合中的实例 `copyDifferentBeans`

>  **BeanUtils.copyDifferentBeans(list,srcClass)**
>
> 1. 将集合中的元素ApiType对象拷贝给ApiTypeView并返回List
> 2. 没有继承关系的bean也可以互相拷贝`(前提bean中的属性声明相同,否则会抛出异常)`
> 3. 当入参和service处理业务的方法参数声明的类型不一样,但属性声明一样的情况下可以使用此方法`(通常用于例如controller 接收前台传来的参数通常是精简后的,但服务端处理业务数据声明一半是全量bean 就会出现类似的问题)`

```javascript

List<ApiTypeView> results = BeanUtils.copyDifferentBeans(list, ApiTypeView.class);
// 结果: [ApiTypeView{typeName='typeName-0', typeCode='typeCode-0', typeParentId=0}, ApiTypeView{typeName='typeName-1', typeCode='typeCode-1', typeParentId=1}...]

```

?> 拷贝实例 `copyDifferentBean`

> BeanUtils.**copyDifferentBean(dist,src)**
>
> 1. 同 **BeanUtils.copyDifferentBeans** 只是针对一个bean进行拷贝

```javascript
ApiType apiType = new ApiType("typeName", "typeCode", 11l, 10, new Date(), "float", "viewFlag", 100l);
ApiTypeView view = new ApiTypeView();
BeanUtils.copyDifferentBean(view, apiType);
//结果: ApiTypeView{typeName='typeName', typeCode='typeCode', typeParentId=11}
```

?> 提取集合中实例的单个属性 `extractFieldByList`

>BeanUtils.**extractFieldByList(list,fieldName)**
>
>1. 将集合中元素的某个属性提取返回Set 
>2. 在开发当中经常遇到将dao返回的list集合中的实体的id返回给list当做下一个dao的查询条件在这种场景下就发挥它的作用减少模版代码
>3. 有两种写法,在下面例子中体现`(为了避免魔法值)`

```javascript
//通过魔法值 'id'
Set<Long> ids = BeanUtils.<Long>extractFieldByList(list, "id");
//通过YFunction 的方式
Set<Long> ids1 = BeanUtils.<Long>extractFieldByList(list, ApiType::getId);
//可以提取元素中任何一个属性
Set<String> typeNames = BeanUtils.<String>extractFieldByList(list, ApiType::getTypeName);
/**
	 结果:
	 [1,2,3...]
	 [1,2,3...]
	 [typeName-4, typeName-5, typeName-6...]
 */
```

?> List 集合转换 map `listToMapByGroupByField` `listToMap`

> BeanUtils.`listToMap(list,fiedId)`
>
> BeanUtils.`listToMapByGroupByField(list,fiedId,filter)`
>
> 1. 将集合元素通过某个属性为Key转换成Map
> 2. 当场景中 有将list转换为mapper映射来规避嵌套循环时,就可以使用到它
> 3. 有多种写法,在下面的例子中体现`(为了避免魔法值)`

```javascript
//将list转换map 元素中id属性作为key,过滤条件为 id不等于null
Map<Long,List<ApiType>> map = BeanUtils.<ApiType,Long,ApiType>listToMapByGroupByField(list,  ApiType::getId, apiType -> apiType.getId() != null);
//将list转换map key为元素中的id id以YFunction的方式传递
Map<Long, ApiType> map1 = BeanUtils.listToMap(list, ApiType::getId);
//将list转换map key位元素中的id  id以魔法值的方式传递
Map<Long, ApiType> map2 = BeanUtils.listToMap(list, "id");
```

?> 将 元集合的某个元素 压入到 目标集合中的某个元素,按照某个条件 `pushOrginFieldToDistBean`

> BeanUtils.pushOrginFieldToDistBean(distList,orginList,distKey,disctVal,orginExpress);
>
> 1. 假设正常情况下 查询了一个用户的 十几笔订单, 通过订单id查询到对应的 订单详情, 正常情况下是要把对应的订单详情压入到订单vo中
>
>    

```javascript
// orderList 为订单列表, Order类型中有订单详情属性 Order.detail 
// detailList 为订单详情列表, OrderDetail类型中有订单id detail.orderId
// Order::getId 为映射的key
// Order::getDetail 为要压入到对应的属性
// OrderDetail::getOrderId 为何 Order::getId 相等的参考值 主外键相等才符合压入的条件
BeanUtils.pushOrginFieldToDistBean(orderList,details,Order::getId,Order::getDetail,OrderDetail::getOrderId);
```



?> 克隆bean `cloneBean`

>BeanUtils.`cloneBean(bean)`
>
>1. 将bean克隆返回一个新的对象 ,对象地址不同
>2. 涉及到在不影响原值的基础上通过原值进行状态修改的场景可以使用

```javascript
//克隆apiType 返回 cloneType
Object cloneType = BeanUtils.cloneBean(apiType);
```

?> 将Bean转换为map `describe`

>BeanUtils.`describe(bean)`
>
>1. 将bean转换为map返回一个Map类型
>2. 这种场景不常用,可以作为了解

```javascript
//转换apiType 为map 
Map<String, String> map = BeanUtils.describe(apiType);
```

?> 将Bean的属性拷贝给新对象 `copyProperties`

> BeanUtils.`copyProperties(dist,src)`
>
> 1. 将bean实例的属性拷贝给新的对象
> 2. 这类似`BeanUtils.cloneBean`

```javascript
//将apiType的属性拷贝给typeView
ApiTypeView typeView = new ApiTypeView();
BeanUtils.copyProperties(typeView,apiType);
```

?> 将map中的对应key覆盖bean实例 `populate`

> BeanUtils.`populate(dist,map)`
>
> 1. 将map.key对应bean中的属性压入到bean中
> 2. 这一般用来做map对bean赋值的场景

```javascript 
//将map中的属性值压入到对应的apiType中
Map<String, Object> map = new HashMap<>();
map.put("typeName", ".......1");
map.put("typeCode", ".......2");
BeanUtils.populate(apiType, map);
```

?> 动态Bean `DynaBean`

> `LazyDynaBean` 
>
> 1. 动态bean可以动态增加属性扩展对象声明时的局限性

```javascript
//声明一个动态bean
DynaBean bean = new LazyDynaBean();
// set 的形式动态添加属性
bean.set("?", "?");
//也可以通过普通的bean来生成一个动态bean扩展属性
DynaBean mybean = new WrapDynaBean(apiType);
mybean.set("float", "self");
//也可以通过 PropertyUtilsBean 表达式的形式获取动态bean中的属性值
PropertyUtilsBean pub = new PropertyUtilsBean();
Object obj = pub.getProperty(mybean, "nama.typeCode");
```

?> bean解析器 `ObjectWrapper`

> 1. 可以通过表达式获取对象中的属性
> 2. 解决一下级联调用的场景(因为 `brick-common` 依赖了beetl模版组件支持更强大的表达式,所以作为了解)

```javascript
//构建对象解析器
ObjectWrapper wrapper = new ObjectWrapper(apiType);
//构建一个map类型属性
Map<String, String> map = new HashMap<>();
map.put("key1", "key1-value");
map.put("key2", "key2-value");
//构建一个list类型的属性
List<String> list = Arrays.asList("s1", "s2", "s3", "s4");
//将属性添加到 解析器中
wrapper.set("otherList", list);
wrapper.set("otherMap", map);
//以下都是对解析器一系列的crud操作
System.out.println("获取 map.key" + wrapper.get("otherMap.key1"));
System.out.println("获取 list[0]：" + wrapper.get("otherList", 1));
System.out.println("添加 map：" + wrapper.get("otherMap", "key1"));
System.out.println("属性是否存在" + wrapper.contains("otherList", null));
wrapper.remove("otherMap", "key1");
System.out.println("测试删除 map 中的 key：" + wrapper.get("otherMap.key1"));
wrapper.set("otherList",3,"other-value");
System.out.println("测试添加 list 的 value：" + wrapper.get("otherList"));
wrapper.set("otherMap", "key3", "value-3");
System.out.println("测试添加 map 中的 kv：" + wrapper.get("otherMap"));
System.out.println(wrapper.get("typeCode"));
```

## 日期操作

?> 常用日期操作 `cn.ifloat.brick.basic.common.toolkit.date.DateUtil`

```javascript
//将date转换str 执行 format
DateUtil.datetimeToStr(new Date(),"yyyy-MM-dd");
//也有约定好默认 日期转换字符串 yyyy-MM-dd HH:mm:ss
DateUtil.dateTimeToStr(new Date());
//默认 日期转换字符串 yyyy-MM-dd
DateUtil.dateToStr(new Date());

//对当前日期做操作
DateUtil.getCurrentDate(); //默认 yyyy-MM-dd
DateUtil.getCurrentDate("yyyy-MM-dd");
DateUtil.getCurrentDatetime(); //默认 yyyy-MM-dd HH:mm:ss

//日子比较
DateUtil.compareTime(new Date(), DateUtil.strToDate("2019-05-01"));

//计算两个日期相差的天数
DateUtil.differentDays(DateUtil.strToDate("2019-05-01"),new Date())
  
//根据日期返回星期几
DateUtil.getWeekStr(new Date())

//判断日期是否是周末
DateUtil.isWeekend(new Date())
  
//返回当前日期的月份有几天
DateUtil.getDayNumsInMonth(new Date())
  
//获取开始和解释时间
DateUtil.getDateTimeBegin(new Date())
DateUtil.getDateTimeEnd(new Date()))
//获取开始和结束时间 返回一个数组 当按照一对的方式时,考虑时间对象的复用率
String[] times = DateUtil.getBeginEndTimeStrs(new Date());

//将日期字符串转换成对应format的日期对象
DateUtil.strToFormatDate("2019-04-21","yyyy-MM-dd")
DateUtil.strToDate("2019-01-02")
DateUtil.strToDateTime("2019-04-03")
  
//获取昨天

//对日期做加减操作
DateUtil.calByDateType(new Date(),10,DateType.MONTH))
 
//获取某年某月的最后一天
DateUtil.getLastDayOfMonth(2019,12)
//获取某年某月的第一天
DateUtil.getBeginDayOfMonth(2019,7)
  

```

## 非空判断

> 当接收到的参数校验非法值时经常会遇到判断为空的情况. 统一按照如下规范

```javascript
//map判断是否为空
MapUtils.isNotEmpty();
MapUtils.isEmpty();
//String判断是否为空
StringUtils.isNotEmpty();
StringUtils.isEmpty();
//集合判断是否为空判断
CollectionUtils.isEmpty();
CollectionUtils.isNotEmpty();
//数组判断是否为空
ArrayUtils.isNotEmpty();
ArrayUtils.isEmpty();
```

