---
title: Intellij IDEA SpringBoot项目热部署解决方案
date: 2017-03-01 11:29:31
tags:
  - java
  - spring
  - spring-boot
categories:
  - spring
  - java
---


1. 在项目pom文件中导入依赖

```xml
<dependency>
    <!--Spring 官方提供的热部署插件 -->
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>1.4.3.RELEASE</version>
</dependency>
```

2. 修改 Intellij IDEA 的配置

`CTRL + SHIFT + A` 查找 `make project automatically` 并选中

`CTRL + SHIFT + A` 查找 `Registry` 找到选项 `compile.automake.allow.when.app.running`

重启`IDEA` 后启动项目就可以在 `IDEA` 中修改代码和静态页面模板，无需再重启 `SpringBoot` 项目了