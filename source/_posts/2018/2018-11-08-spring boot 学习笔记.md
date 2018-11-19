---
title: spring boot 入门笔记
categories: [Java, Java web]
comments: true
tags: Java
---

[B站上学Spring Boot](https://www.bilibili.com/video/av20965295/?p=6)

## 写一个Hello World程序

1. 创建Maven项目
2. 导入spring boot相关的依赖
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.5.RELEASE</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
3. 编写一个主程序，用来启动spring boot应用
```java
/**
 * @SpringBootApplication 来标注一个主程序，这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        //Spring 应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }

}
```
4. 编写相关的Controller，Service
```java
@Controller
@RequestMapping({"/hello"})
public class HelloController {

    @ResponseBody
    @RequestMapping({"/hello"})
    public String hello(){
        return "hello";
    }
}
```
5. 运行主程序测试
6. 简化部署
```xml
<!--这个插件可以将应用打包成一个可执行的jar包-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```
将这个应用打成jar包，直接使用`java -jar xxx.jar`运行。使用`jar xf xxx.jar`可以查看其中的内容。“x”是“提取”的缩写。“f”是“文件”的缩写。放在一起，“xf”代表你想要在命令行里提取特定的文件。

## Hello World 研究

### POM文件

1. 父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.5.RELEASE</version>
</parent>

按住command点spring-boot-starter-parent，点进去发现spring-boot-starter-parent它的父项目是：
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.5.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>

在spring-boot-dependencies中定义了大量依赖的版本号，它来真正管理Spring Boot应用中的所有依赖版本：
<properties>
    <activemq.version>5.15.6</activemq.version>
    <antlr2.version>2.7.7</antlr2.version>
    <appengine-sdk.version>1.9.64</appengine-sdk.version>
    <artemis.version>2.4.0</artemis.version>
    <aspectj.version>1.8.13</aspectj.version>
    <assertj.version>3.9.1</assertj.version>
    ...
```

所以有了这个Spring Boot的版本仲裁中心，以后我们导入依赖默认是不需要写版本号的（没有在spring-boot-dependencies中管理的，自然需要我们自己声明版本号）。

2. 导入的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

点击进spring-boot-starter-web，把鼠标放到打开的标签页上，可以看到这个pom文件的路径，可以看到是在`.m2/repository/`下的，路径由：`groupId`、`artifactId`、`version`三者组成（这里省略了version，使用spring-boot-dependencies中定义的默认版本号）。

查看spring-boot-starter-web的依赖，可以看到：**spring-boot-starter**: spring-boot场景启动器，帮我们导入了web模块正常运行所依赖的组件。

[Starters](https://docs.spring.io/spring-boot/docs/2.1.0.RELEASE/reference/htmlsingle/#using-boot-starter)

Spring Boot 将所有的功能场景都抽取出来了，做成一个个的starters（启动器），只需要在项目里面引入这些starter，相关场景的所有依赖都会导入进来。要用什么功能，就导入什么场景的启动器。

### 主程序类，主入口类

```java
/**
 * @SpringBootApplication 来标注一个主程序，这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        //Spring 应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }

}
```
**@SpringBootApplication**：标注在某个类上，说明这个类是Spring Boot的主配置类。Spring Boot就应该运行这个类的main方法来启动SpringBoot应用。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

**@SpringBootConfiguration**: Spring Boot的配置类。然后再点进去查看它，看到 **@Configuration**：spring标注在配置类上。配置类 - - 配置文件。配置类也是容器中的一个组件（@Component）。

**@EnableAutoConfiguration**：开启自动配置。以前我们需要在spring中配置的东西，现在spring boot帮我们配置。

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```
- **@AutoConfigurationPackage**: 自动配置包。
    - **@Import({Registrar.class})**：spring的底层注解@Import，给容器导入一个组件。`Registrar`将主配置类所在的包及下面的子包里面的所有组件都扫描到spring容器中。
- **@Import({AutoConfigurationImportSelector.class})**：`AutoConfigurationImportSelector`导入哪些组件的选择器。`selectImports`将所有需要导入的组件以全类名的方式返回。这些组件就会被添加到容器中。会给容器中导入非常多的自动配置类（xxxAutoConfiguration）

![自动配置类](../../../../images/2018/自动配置类.png)

有了自动配置类，免去了我们手动编写配置注入功能组件等的工作

spring boot在启动的时候从类路径下的`META-INF/spring.factories`中获取EnableAutoConfiguration指定的这些值。

J2EE的整体整合解决方案和自动配置都在`spring-boot-autoconfigure-2.0.5.RELEASE.jar`中。

## 使用Spring Initializer快速创建Spring Boot项目

IDE都支持使用spring的项目创建向导快速创建一个Spring Boot项目。

选择我们需要的模块，向导会联网创建Spring Boot项目。

默认生成的Spring Boot项目，主程序已经生成好了，我们只需要编写我们自己的逻辑。

resource文件夹中目录结构：

- static目录: 保存静态资源：js css images
- templates目录: 保存所有的模板页面，Spring Boot默认jar包使用嵌入式的tomcat，不支持jsp；可以使用模板引擎（freemarker, thymeleaf）
- application.properties文件: Spring Boot 的应用配置文件，可以修改一些默认配置。



