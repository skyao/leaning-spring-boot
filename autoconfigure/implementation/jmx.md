# JMX

根据配置属性自动配置启用/禁用 Spring 的 `EnableMBeanExport` 机制。

设置 `spring.jmx.enabled` 为 false 来禁用注解 bean 的自动导出。


JmxAutoConfiguration 的定义：

```java
@Configuration
@ConditionalOnClass({ MBeanExporter.class })
@ConditionalOnProperty(prefix = "spring.jmx", name = "enabled", havingValue = "true", matchIfMissing = true)
public class JmxAutoConfiguration implements EnvironmentAware, BeanFactoryAware {}
```

自动配置的前提：

- classpath 中存在类 MBeanExporter.class
- 属性 `spring.jmx.enabled` 值为true，或者没有配置

默认配置的 bean：

- AnnotationMBeanExporter
- ParentAwareNamingStrategy
- MBeanServer

配置方式是典型的使用 `@ConditionalOnMissingBean`，如果需要替换这里的默认配置，用户只要自行定义好自己的对应 bean 。

```java
@Bean
@ConditionalOnMissingBean(value = ObjectNamingStrategy.class, search = SearchStrategy.CURRENT)
public ParentAwareNamingStrategy objectNamingStrategy() {}
```


