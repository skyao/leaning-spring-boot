@Value 注解
===========

字段或者方法/构造函数参数级别的注解，表明被影响的参数的默认值表达式。

典型用于表达式驱动的依赖注入。也支持handler方法参数的动态解析，如在spring MVC中。

通常的使用场景是使用"#{systemProperties.myProp}"风格的表达式来给字段赋值默认值。

注意：@Value 注解的实际处理是 BeanPostProcessor 执行的，这意味着你 **无法** 在 BeanPostProcessor 或者 BeanFactoryPostProcessor 类习惯中中使用 @Value。

### 源代码

@Value 注解的源代码：

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Value {
	String value();
}
```


