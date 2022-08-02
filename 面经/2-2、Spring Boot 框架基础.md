# 2-2　Spring Boot 框架基础

## 1、使用 Spring Boot 的原因

### 1.1 Spring 的不足

Spring 的组件代码是轻量级的，但它的配置却是重量级的（需要大量 XML 配置） 。

### 1.2 Spring Boot 的优点

Spring 旨在简化 J2EE 企业应用程序开发。

Spring Boot  旨在简化 Spring 开发（减少配置文件，开箱即用！）。

1. 开发基于 Spring 的应用程序很容易。
2. Spring Boot 项目所需的开发或工程时间明显减少，通常会提高整体生产力。
3. Spring Boot **不需要编写大量样板代码、XML 配置和注释**。
4. Spring 引导应用程序可以很容易地与 Spring 生态系统集成，如 Spring JDBC、Spring ORM、Spring Data、Spring Security 等。
5. Spring Boot 遵循“固执己见的默认配置”，即**约定大于配置，减少开发工作**（默认配置可以修改）。
6. Spring Boot 应用程序**提供嵌入式 HTTP 服务器**，如 Tomcat 和 Jetty，可以轻松地开发和测试 web 应用程序。（这点很赞！普通运行 Java 程序的方式就能运行基于 Spring Boot web 项目，省事很多）
7. Spring Boot 提供命令行接口(CLI)工具，用于开发和测试 Spring Boot 应用程序，如 Java 或 Groovy。
8. Spring Boot 提供了多种插件，可以使用内置工具(如 Maven 和 Gradle)开发和测试 Spring Boot 应用程序



##  Spring Boot Starters

Spring Boot Starters 是一系列依赖关系的集合，因为它的存在，项目的依赖之间的关系对我们来说变的更加简单了。

## @SpringBootApplication 注解

在 IDEA 中 `XXXApplication.java` 启动类中，进入 `@SpringBootApplication` 注解，可以看到其详细实现：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
    ……
}
```

然后继续进入上面的 `@SpringBootConfiguration` 注解，可以看到其实现如下：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
@Indexed
public @interface SpringBootConfiguration {
    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

由上面的实现看出，`@SpringBootApplication`是 `@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan` 注解的集合。

**根据 SpringBoot 官网，这三个注解的作用：**

+ `@EnableAutoConfiguration`：启用 SpringBoot 的自动配置机制；
+ `@ComponentScan`：扫描被 `@Component`（`@Service`，`@Controller`）注解的 `bean`，注解默认会扫描该类所在的包下所有的类；
+ `@Configuration`：允许在上下文中注册额外的 `bean` 或导入其他配置类。

**`@EnableAutoConfiguration` 是启动自动配置的关键**。

