# @EnableAutoConfiguration

## 介绍

启用 Spring 应用程序上下文的自动配置，尝试猜测并配置您可能需要的bean。自动配置类通常根据您的 classpath 和您已经定义的bean来实现。例如，如果您的 classpath 中有 tomcat-embedded.jar，则可能需要一个TomcatEmbeddedServletContainerFactory（除非您已经定义了自己的 EmbeddedServletContainerFactory bean）。

自动配置尝试尽可能的智能化，并在当您定义更多的自己的配置时回退。您可以随时手动 `exclude()` 任何您不想应用的配置（如果您不能访问他们，请使用 `excludeName()`）。您也可以通过 `spring.autoconfigure.exclude` 属性排除它们。自动配置总是在用户定义的 bean 注册之后实施。

用 `@EnableAutoConfiguration` 注解的类的 package 具有特定的意义，通常用作“默认”。例如，它将在扫描 `@Entity` 类时使用。通常建议您将 `@EnableAutoConfiguration` 放在 root package 中，以便可以搜索所有子 package 和类。

自动配置类是常规的 Spring 配置bean。它们使用 `SpringFactoriesLoader` 机制（以类作为Key）进行查找。通常，自动配置 bean 是 `@Conditional` bean（通常使用 `@ConditionalOnClass` 和 `@ConditionalOnMissingBean` 注解）。

## 代码实现

### annotation 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

- Target: TYPE，用于类
- Retention： RUNTIME,编译和运行时可见
- Documented： 有文档要求
- Inherited： 可以继承
- AutoConfigurationPackage：表示包含这个注解的 package 应该被注册到 `AutoConfigurationPackages`

### annotation import

import 了 EnableAutoConfigurationImportSelector：

```java
......
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

EnableAutoConfigurationImportSelector 中定义了一个 isEnabled() 方法：

```java
@Override
protected boolean isEnabled(AnnotationMetadata metadata) {
    if (getClass().equals(EnableAutoConfigurationImportSelector.class)) {
        return getEnvironment().getProperty(
                EnableAutoConfiguration.ENABLED_OVERRIDE_PROPERTY, Boolean.class,
                true);
    }
    return true;
}
```

ENABLED_OVERRIDE_PROPERTY 配置项可以用来覆盖配置来开启/关闭自动配置的功能。

```java
public @interface EnableAutoConfiguration {
	String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
}
```

### annotation 内容

如果希望排除某些不想自动配置的内容，可以使用 exclude() 和 excludeName() 方法：

```java
public @interface EnableAutoConfiguration {
	/**
	 * 排除特定的自动配置类，使其不会被应用。
	 * @return the classes to exclude
	 */
	Class<?>[] exclude() default {};

	/**
	 * 排除特定的自动配置类名，使其永远不会被应用。
	 * @return the class names to exclude
	 * @since 1.3.0
	 */
	String[] excludeName() default {};
}
```

