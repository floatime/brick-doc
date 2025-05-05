# PinYinKit 组件

> 前言
>
> 该组件依赖于huTool包
>
> 引用该组件以实现汉字拼音获取及首字母获取



## 引入依赖

```xml
<dependency>
     <groupId>cn.ifloat.brick</groupId>
     <version>1.0.0-SNAPSHOT</version>
     <artifactId>brick-chinese</artifactId>
</dependency>
```



## 使用

> PinYinKit共提供了四个方法:

> 获取汉字全拼:
>
> 方法入参:要转为拼音的字符串,汉字之间的分隔符

```java
String name="测试人员";
String str = PinYinKit.full(name, "_");
System.out.println(str);
//打印结果为:  ce_shi_ren_yuan
```

> 获取汉字首字母(小写):
>
> 方法入参:要转为拼音的字符串,汉字之间的分隔符

```java
String name="测试人员";
String str = PinYinKit.acronym(name, "_");
System.out.println(str);
//打印结果为:  c_s_r_y
```

> 获取汉字首字母(大写):
>
> 方法入参:要转为拼音的字符串,汉字之间的分隔符

```java
String name="测试人员";
String str = PinYinKit.acronym(name, "_");
System.out.println(str);
//打印结果为:  C_S_R_Y
```

> 获取汉字全拼(带音调):
>
> 方法入参:要转为拼音的字符串,汉字之间的分隔符

```java
String name="测试人员";
String str = PinYinKit.fullWithTonl(name, "_");
System.out.println(str);
//打印结果为:  ce4_shi4_ren2_yuan2
```

