# AOP

自动配置 Spring 的 AOP 支持。 相当于在您的配置中启用 `EnableAspectJAutoProxy`。

如果 `spring.aop.auto = false`，配置将不会被激活。默认情况下，`proxyTargetClass` 属性为false，但可以通过指定 `spring.aop.proxyTargetClass = true` 来覆盖。

AopAutoConfiguration 的定义：

```java
@Configuration
@ConditionalOnClass({ EnableAspectJAutoProxy.class, Aspect.class, Advice.class })
@ConditionalOnProperty(prefix = "spring.aop", name = "auto", havingValue = "true", matchIfMissing = true)
public class AopAutoConfiguration {}
```

AOP自动配置的前置条件：

- classpath 中存在类 EnableAspectJAutoProxy / Aspect / Advice
- 属性 `spring.aop.auto` 值为true，或者没有配置

spring AOP 有两种实现方式：

1. Jdk Dynamic AutoProxy
2. Cglib AutoProxy

可以通过配置项 `spring.aop.proxy-target-class` 控制：

```java
@Configuration
@EnableAspectJAutoProxy(proxyTargetClass = false)
@ConditionalOnProperty(prefix = "spring.aop", name = "proxy-target-class", havingValue = "false", matchIfMissing = true)
public static class JdkDynamicAutoProxyConfiguration {}

@Configuration
@EnableAspectJAutoProxy(proxyTargetClass = true)
@ConditionalOnProperty(prefix = "spring.aop", name = "proxy-target-class", havingValue = "true", matchIfMissing = false)
public static class CglibAutoProxyConfiguration {}
```

- 当 `spring.aop.proxy-target-class` 未配置或者配置为"false"时，启动 JdkDynamicAutoProxyConfiguration
- 当 `spring.aop.proxy-target-class` 配置为"true"时，启动 CglibAutoProxyConfiguration

> 注： 这相当于在自动配置中实现了一个 `if / else` 的逻辑。

