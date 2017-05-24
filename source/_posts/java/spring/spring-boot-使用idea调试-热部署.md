---
title: spring-boot 使用idea调试，热部署
date: 2017-03-01 11:14:06
tags:
  - java
  - spring
  - spring-boot
categories:
  - spring
  - java
---


## spring-boot 热部署功能


加 maven 依赖 

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-devtools</artifactId>  
    <optional>true</optional>  
</dependency>
```

开启热部署

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## IDEA 自动编译

1. `CTRL + SHIFT + A`  --> 查找 `make project automatically` --> 选中

2. `CTRL + SHIFT + A` --> 查找 `Registry` --> 找到并勾选 `compiler.automake.allow.when.app.running`