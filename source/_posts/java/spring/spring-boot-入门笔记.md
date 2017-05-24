---
title: spring-boot-入门笔记
date: 2017-03-01 10:11:26
tags:
  - java
  - spring
categories:
  - spring
  - java
---


spring 官方文档 http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-documentation


使用 IDEA 创建 spring-boot web 项目，目录结构如下

```
├─.idea
│  └─libraries
├─.mvn
│  └─wrapper
├─src
│  ├─main
│  │  ├─java
│  │  │  └─com
│  │  │      └─example
│  │  └─resources
│  │      ├─static
│  │      └─templates
│  └─test
│      └─java
│          └─com
│              └─example
└─target
    ├─classes
    │  └─com
    │      └─example
    ├─generated-sources
    │  └─annotations
    ├─generated-test-sources
    │  └─test-annotations
    └─test-classes
        └─com
            └─example
```

进入 `demo\src\main\java\com\example\`  目录

右键新建 `Hello Class` 修改代码如下

```java
/**
 * Created by Julian on 2017/3/1.
 */
@RestController
public class Hello {
    @RequestMapping("/")
    String index(){
        return "Hello word";
    }
}
```

本文根据官方文档深入讲解一段代码

Spring Boot 建议使用 Maven 或 Gradle ，本文以 Maven 为例。

首先创建一个一般的 Maven 项目，有一个 `pom.xml` 和基本的 `src/main/java` 结构。

在 `pom.xml` 中写上如下内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.github.abel533</groupId>
    <artifactId>spring-boot</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.3.0.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework</groupId>
                        <artifactId>springloaded</artifactId>
                        <version>1.2.5.RELEASE</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
</project>
```


首先是增加了 `<parent>`


增加父 `pom` 比较简单，而且 `spring-boot-starter-parent` 包含了大量配置好的依赖管理，在自己项目添加这些依赖的时候不需要写 `<version>` 版本号。


使用父 `pom` 虽然简单，但是有些情况我们已经有父 `pom` ，不能直接增加 `<parent>` 时，可以通过如下方式：

```xml
<dependencyManagement>
     <dependencies>
        <dependency>
            <!-- Import dependency management from Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.2.3.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```


`java.version` 属性

上面 `pom.xml` 虽然没有出现这个属性，这里要特别提醒。

`Spring` 默认使用 `jdk1.6` ，如果你想使用 `jdk1.8` ，你需要在 `pom.xml` 的属性里面添加 `java.version` ，如下：

```xml
<properties>
    <java.version>1.8</java.version>
</properties>
```

添加 `spring-boot-starter-web` 依赖

` Spring` 通过添加 `spring-boot-starter-*` 这样的依赖就能支持具体的某个功能。

我们这个示例最终是要实现 `web` 功能，所以添加的是这个依赖。

更完整的功能列表可以查看：[using-boot-starter-poms](http://docs.spring.io/spring-boot/docs/1.2.3.RELEASE/reference/html/using-boot-build-systems.html#using-boot-starter-poms)


添加 `spring-boot-maven-plugin` 插件

该插件支持多种功能，常用的有两种，第一种是打包项目为可执行的 `jar` 包。

在项目根目录下执行 `mvn package` 将会生成一个可执行的jar包，`jar` 包中包含了所有依赖的jar包，

只需要这一个 `jar` 包就可以运行程序，使用起来很方便。该命令执行后还会保留一个 `XXX.jar.original` 的 `jar` 包，包含了项目中单独的部分。

生成这个可执行的 `jar` 包后，在命令行执行 `java -jar xxxx.jar` 即可启动项目。

另外一个命令就是 `mvn spring-boot:run` ，可以直接使用 `tomcat`（默认）启动项目。


在我们开发过程中，我们需要经常修改，为了避免重复启动项目，我们可以启用热部署。

`Spring-Loaded` 项目提供了强大的热部署功能，添加/删除/修改 方法/字段/接口/枚举 等代码的时候都可以热部署，速度很快，很方便。

想在 `Spring Boot` 中使用该功能非常简单，就是在 `spring-boot-maven-plugin` 插件下面添加依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>springloaded</artifactId>
        <version>1.2.5.RELEASE</version>
    </dependency>
</dependencies>
```

添加以后，通过 `mvn spring-boot:run` 启动就支持热部署了。

> 注意：使用热部署的时候，需要IDE编译类后才能生效，你可以打开自动编译功能，这样在你保存修改的时候，类就自动重新加载了。


### 创建一个应用类


我们创建一个 `Application` 类：

```java
@RestController
@EnableAutoConfiguration
public class Application {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    @RequestMapping("/now")
    String hehe() {
        return "现在时间：" + (new Date()).toLocaleString();
    }

    public static void main(String[] args) {
        SpringApplication.run(Example.class, args);
    }

}
```

> 注意 `Spring Boot` 建议将我们 `main` 方法所在的这个主要的配置类配置在根包名下。

类似如下结构：

```
com
 +- example
     +- myproject
         +- Application.java
         |
         +- domain
         |   +- Customer.java
         |   +- CustomerRepository.java
         |
         +- service
         |   +- CustomerService.java
         |
         +- web
             +- CustomerController.java
```


在 `Application.java` 中有 `main` 方法。

因为默认和包有关的注解，默认包名都是当前类所在的包，例如 `@ComponentScan`, `@EntityScan`, `@SpringBootApplication` 注解。


#### `@RestController`

因为我们例子是写一个web应用，因此写的这个注解，这个注解相当于同时添加 `@Controller` 和 `@ResponseBody` 注解。


#### `@EnableAutoConfiguration`

`Spring Boot` 建议只有一个带有该注解的类。

`@EnableAutoConfiguration` 作用：`Spring Boot` 会自动根据你 `jar` 包的依赖来自动配置项目。

例如当你项目下面有 `HSQLDB` 的依赖时，`Spring Boot` 会创建默认的内存数据库的数据源 `DataSource`，

如果你自己创建了 `DataSource` ，`Spring Boot` 就不会创建默认的 `DataSource`。


如果你不想让 `Spring Boot` 自动创建，你可以配置注解的 `exclude` 属性，例如：

```java
@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
    //...
}
```

#### `@SpringBootApplication`

由于大量项目都会在主要的配置类上添加 `@Configuration` ,`@EnableAutoConfiguration` , `@ComponentScan` 三个注解。

因此 `Spring Boot` 提供了 `@SpringBootApplication` 注解，该注解可以替代上面三个注解（使用 `Spring` 注解继承实现）。


#### home等方法

```java
@RequestMapping("/")
String home() {
    return "Hello World!";
}

@RequestMapping("/now")
String hehe() {
    return "现在时间：" + (new Date()).toLocaleString();
}
```

这些方法都添加了 `@RequestMapping("xxx")` ，这个注解起到路由的作用。


### 启动项目 SpringApplication.run


启动 `Spring Boot` 项目最简单的方法就是执行下面的方法：

```java
SpringApplication.run(Application.class, args);
```


该方法返回一个 `ApplicationContext` 对象，

使用注解的时候返回的具体类型是 `AnnotationConfigApplicationContext`

或 `AnnotationConfigEmbeddedWebApplicationContext` ，当支持 web 的时候是第二个。


除了上面这种方法外，还可以用下面的方法：

```java
SpringApplication application = new SpringApplication(Application.class);
application.run(args);
```


`SpringApplication` 包含了一些其他可以配置的方法，如果你想做一些配置，可以用这种方式。

除了上面这种直接的方法外，还可以使用 `SpringApplicationBuilder` ：

```java
new SpringApplicationBuilder()
        .showBanner(false)
        .sources(Application.class)
        .run(args);
```

当使用 `SpringMVC` 的时候由于需要使用子容器，

就需要用到 `SpringApplicationBuilder`， 该类有一个 `child(xxx...)` 方法可以添加子容器。


### 运行


在IDE中直接直接执行 `main` 方法，然后访问 `http://localhost:8080` 即可。

另外还可以用上面提到的 `mvn` ，可以打包为可执行jar包，然后执行 `java -jar xxx.jar` 。

或者执行 `mvn spring-boot:run` 运行项目。


项目启动后输出如下日志：


```
com.intellij.rt.execution.application.AppMain com.example.DemoApplication

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.5.1.RELEASE)

2017-03-01 10:23:16.701  INFO 8468 --- [           main] com.example.DemoApplication              : Starting DemoApplication on DESKTOP-98DOLMD with PID 8468 
```


以上是 `Spring Boot` 基础的内容，

有些不全面的地方或者读者有更多疑问，

可以查看 [Spring Boot完整文档](http://docs.spring.io/spring-boot/docs/1.2.3.RELEASE/reference/html/index.html)。