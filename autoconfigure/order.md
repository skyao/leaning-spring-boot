# 顺序

当有多个自动配置类时，如何控制彼此之间的顺序？

## @AutoConfigureOrder

Spring Framework 的 `Order` 注解的自动配置特定变量。允许自动配置类之间进行排序，而不会影响配置类传递到 `AnnotationConfigApplicationContext.register(Class...)`的顺序.

```java
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
```

### Ordered 定义

Ordered 的值实际是 int 类型，值越小，代表最高的优先级。

```java
public interface Ordered {
	/**
	 * 最高优先级值的常用常数
	 */
    int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;
    /**
	 * 最低优先级值的常用常数
	 */
    int LOWEST_PRECEDENCE = Integer.MAX_VALUE;

    int getOrder();
}
```

## @AutoConfigureAfter

提示自动配置应在其他指定的自动配置类之后应用。

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.TYPE })
public @interface AutoConfigureAfter {

	/**
	 * 应该已经应用过的自动配置的类
	 * @return the classes
	 */
	Class<?>[] value() default {};

	/**
	 * 应该已经应用过的自动配置的类的名字
	 * @return the class names
	 * @since 1.2.2
	 */
	String[] name() default {};

}
```

可以直接指定自动配置类的 class 或者，如果不能访问这个class，则可以通过 name() 指定自动配置类的 java 全限定名.示例如下：

```java
@AutoConfigureAfter(TransactionConfiguration.class)
@AutoConfigureAfter(JpaConfiguration.class)
@AutoConfigureAfter(name = "org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration")
```

## @AutoConfigureBefore

提示自动配置应在其他指定的自动配置类之前应用。

使用方式同 @AutoConfigureAfter

