@EnableConfigurationProperties注解
=================================

## 使用方式

开启对 @ConfigurationProperties 注解标记的bean的支持。

@ConfigurationProperties 的bean可以以标准方式被注册(例如使用 @Bean 方法)， 或者，为了方便起见,可以直接在这个注解上指定。

下面的例子，通过在@EnableConfigurationProperties注解中直接简单的列出ConnectionSettings.class 来快捷的注册@ConfigurationProperties bean的定义。

```java
@Configuration
@EnableConfigurationProperties(ConnectionSettings.class)
public class MyConfiguration {
}
```

### 源代码

@EnableConfigurationProperties 注解的源代码如下：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(EnableConfigurationPropertiesImportSelector.class)
public @interface EnableConfigurationProperties {
	Class<?>[] value() default {};
}
```

value属性是Class<?>[]，这意味着可以同时注册多个@ConfigurationProperties。
