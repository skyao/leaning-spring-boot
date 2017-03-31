# 条件

自动配置的一个重要特性就是当用户有自己的配置时，自动配置就会退后。只有当用户没有进行配置时，自动配置才开始生效。这样就需要控制自动配置中的每个配置在什么情况下生效，所谓条件/Condition。

### @ConditionalOnBean

仅当 BeanFactory 中已经包含指定的 bean class 和/或 name 时才匹配条件。

该条件只能匹配到目前为止 application context 已经处理的 bean 定义，因此强烈建议仅在自动配置类上使用此条件。如果候选 bean 可能由另一个自动配置创建，请确保使用此 condition 的自动配置类在其后运行。


```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(OnBeanCondition.class)
public @interface ConditionalOnBean {

	/**
	 * 需要检查的 bean 的 class 类型。当 ApplicationContext 包含任意一个指定的 class 时条件匹配。
	 */
	Class<?>[] value() default {};

	/**
	 * 需要检查的 bean 的 class 类型名称(java 全限定名)。当 ApplicationContext 包含任意一个指定的 class 时条件匹配。
	 */
	String[] type() default {};

	/**
	 * 装饰需要检查的 bean 的注解. 当 ApplicationContext 包含任意一个带有这个注解的 bean 时条件匹配。
	 */
	Class<? extends Annotation>[] annotation() default {};

	/**
	 * 需要检查的 bean 的 name。当 ApplicationContext 包含任意一个指定的 class 时条件匹配。
	 */
	String[] name() default {};

	/**
	 * 决定是否应考虑 application context 层次（parent contexts）的策略。
	 */
	SearchStrategy search() default SearchStrategy.ALL;

}
```

使用示例如下：

```java
@ConditionalOnBean(PlatformTransactionManager.class)
@ConditionalOnBean(DataSource.class)
@ConditionalOnBean(EntityManagerFactory.class)
```

### @ConditionalOnClass

仅当 classpath 上存在指定类时条件匹配。

使用示例如下：

```java
@ConditionalOnClass(SpringAxonAutoConfigurer.class)
@ConditionalOnClass(PlatformTransactionManager.class)
@ConditionalOnClass(SpringAMQPPublisher.class)
@ConditionalOnClass(name = {"org.axonframework.jgroups.commandhandling.JGroupsConnector", "org.jgroups.JChannel"})
```

### @ConditionalOnCloudPlatform

仅当指定的云平台处于活动状态时条件匹配。

目前支持 CLOUD_FOUNDRY 和 HEROKU。

### @ConditionalOnExpression

依赖于 `SpEL` 表达式的值的条件元素的配置注解。

### @ConditionalOnJava

基于应用运行的 JVM 版本的条件匹配。

### @ConditionalOnJndi

基于 JNDI InitialContext 的可用和可以查找指定位置的条件匹配。

### @ConditionalOnMissingBean

仅当 `BeanFactory` 中不包含指定的 bean class 和/或 name 时条件匹配。

该条件只能匹配到目前为止 application context 已经处理的 bean 定义，因此强烈建议仅在自动配置类上使用此条件。如果候选 bean 可能由另一个自动配置创建，请确保使用此 condition 的自动配置类在其后运行。

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(OnBeanCondition.class)
public @interface ConditionalOnMissingBean {

	/**
	 * 需要检查的 bean 的 class 类型。当 ApplicationContext 不包含每一个被指定的 class 时条件匹配。
	 */
	Class<?>[] value() default {};

	/**
	 * 需要检查的 bean 的 class 类型名称(Java全限定名)。当 ApplicationContext 不包含每一个被指定的 class 时条件匹配。
	 */
	String[] type() default {};

	/**
	 * 识别匹配 bean 时，可以被忽略的 bean 的 class 类型
	 */
	Class<?>[] ignored() default {};

	/**
	 * 识别匹配 bean 时，可以被忽略的 bean 的 class 类型名称(Java全限定名)
	 */
	String[] ignoredType() default {};

	/**
	 * 装饰需要检查的 bean 的注解。当 ApplicationContext 不包含带有这些注解的 bean 时条件匹配。
	 */
	Class<? extends Annotation>[] annotation() default {};

	/**
	 * 需要检查的 bean 的 name。当 ApplicationContext 不包含任意指定的每一个的 class 时条件匹配。
	 */
	String[] name() default {};

	/**
	 * 决定是否应考虑 application context 层次（parent contexts）的策略。
	 */
	SearchStrategy search() default SearchStrategy.ALL;

}
```

使用示例如下：

```java
@ConditionalOnMissingBean
@ConditionalOnMissingBean(TransactionManager.class)
@ConditionalOnMissingBean(ignored = {DistributedCommandBus.class})
@ConditionalOnMissingBean({EventStorageEngine.class, EventBus.class, EventStore.class})
```

### @ConditionalOnMissingClass

仅当 classpath 上不存在指定类时条件匹配。

### @ConditionalOnNotWebApplication

仅当 application context 不是 web application context 时条件匹配。

### @ConditionalOnProperty

条件是检查指定的属性是否具有指定的值。默认情况下，属性必须存在于环境(Environment)中，且不等于false。 havingValue()和matchIfMissing()属性允许进一步的定制化。

havingValue()属性可用于指定属性应具有的值。下表显示根据属性值和havingValue()属性何时条件匹配：

| Property Value | havingValue="" |havingValue="true" |havingValue="false" |havingValue="foo" |
|--------|--------|--------|--------|--------|
| "true" |  yes   |  yes   |  no    |   no   |
| "false"|  no    |  no    | yes    |   no   |
| "foo"  |  yes   |  no    |  no    |  yes   |

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.TYPE, ElementType.METHOD })
@Documented
@Conditional(OnPropertyCondition.class)
public @interface ConditionalOnProperty {

	/**
	 * name()的别名.
	 */
	String[] value() default {};

	/**
	 * 应用于每个属性的前缀。如果未指定，前缀将自动以点号结束(也就是说如果前缀不是以点号结束则会自动加上一个点号)。
	 */
	String prefix() default "";

	/**
	 * 要测试的属性的名称。如果已经定义了前缀，它将被应用于计算每个属性的完整 key。 例如，如果前缀是 app.config 而值是 my-value，那么完整的 key 将是 app.config.my-value
使用虚线表示法来指定每个属性，即全部字母小写，用"-"来分隔单词（例如 my-long-property ）。
	 */
	String[] name() default {};

	/**
	 * 属性期望值的字符串表示形式。如果未指定，该属性必须不能为 false。
	 */
	String havingValue() default "";

	/**
	 * 指定当属性未设置时，条件是否匹配。默认为false。
	 */
	boolean matchIfMissing() default false;

	/**
	 * 如果应该检查 relaxed name。默认为true。
	 */
	boolean relaxedNames() default true;

}
```

使用示例如下：

```java
@ConditionalOnProperty("axon.amqp.exchange")
@ConditionalOnProperty("axon.distributed.jgroups.enabled")
```

### @ConditionalOnResource

仅当 classpath 上存在指定资源时条件匹配。

### @ConditionalOnSingleCandidate

仅当 BeanFactory 中包含指定的 bean 类并且可以判断只有单个候选者时条件匹配。

如果 BeanFactory 中已经包含多个匹配的 bean 实例，但是已经定义了一个主要候选者，条件也将匹配。基本上，如果用已定义类型自动装配 bean 条件匹配将成功。

该条件只能匹配到目前为止 application context 已经处理的 bean 定义，因此强烈建议仅在自动配置类上使用此条件。如果候选bean可能由另一个自动配置创建，请确保使用此 condition 的自动配置类在其后运行。

### @ConditionalOnWebApplication

仅当 application context 是 web application context 时条件匹配。



