### Maven 获取方式

?> **Tip** 目前只能通过内部Maven私服来获取 

#### 当前版本

> 目前版本还未发布 当前版本为 1.0.0-SNAPSHOT

#### 基础依赖

##### brick-spring-profile-common

?> packageing:pom  SpringBoot & SpringCloud 项目结构下的父依赖 

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-spring-profile-common</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-basic-base

?> packageing:pom 非Spring环境下的基础父依赖

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-basic-base</artifactId>
  <version>${brick.version}</version>
</dependency>
```

#### 非Spring环境下的基础组件

##### brick-basic-common

?>**brick-basic-common** packageing: jar  包含反射 & 缓存 & 表达式解析 & bean动态操作 & 日期操作 & 基础声明定义

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-basic-common</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-httpclients

?>**brick-httpclients** packageing: jar  常用httpclient相关操作  httpclients 组件

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-httpclients</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-logger

?>**brick-logger** packageing: jar  常用日志操作

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-logger</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-poi

?>**brick-poi** packageing: jar  office 相关操作

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-poi</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-redis

?>**brick-redis** packageing: jar  redis 相关操作

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-redis</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-security-common

?>**brick-security-common** packageing: jar  加密 & 解密 & 编码 & 解码相关操作

```xml
<dependency>
  <groupId>cn.ifloat.brick</groupId>
  <artifactId>brick-security-common</artifactId>
  <version>${brick.version}</version>
</dependency>
```

#### Spring环境下的组件

!> 所谓的Spring环境下的组件就是基础组件基础上加上了一些自动注入的配置项,方便在Spring环境下随Ioc Aop方式自动装配

##### brick-sprofile-betags

?> **brick-sprofile-betags** 封装了beetl的以及自定义标签的组件,方便处理传统类似 JSP开发模式的场景

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-betags</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-common

?> **brick-sprofile-common**  基于Spring Ioc Aop 实现的可自动装配的处理类和获取Spring上下文的工具集

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-common</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-cloud-common

?> **brick-sprofile-cloud-common**  基于SpringCloud环境增加的一些工具集

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-cloud-common</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-httpclients

?> **brick-sprofile-httpclients**  封装 `brick-httpclients` 加入自动装配

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-httpclients</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-mybatis

?> **brick-sprofile-mybatis**  对`mybatisplus` 进行了封装

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-mybatis</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-redis

?> **brick-sprofile-redis**  封装`brick-redis` 加入自动装配

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-redis</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-web

?> **brick-sprofile-web**  加入对`SpringMvc`定制规则

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-web</artifactId>
  <version>${brick.version}</version>
</dependency>
```

##### brick-sprofile-cloud

?> **brick-sprofile-cloud**  加入对`SpringCloud`定制规则

```xml
<dependency>
  <groupId>cn.ifloat.brick.sprofile</groupId>
  <artifactId>brick-sprofile-cloud</artifactId>
  <version>${brick.version}</version>
</dependency>
```

