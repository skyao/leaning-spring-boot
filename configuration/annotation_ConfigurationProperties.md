@ConfigurationProperties 注解
============================

用于外部化(externalized)配置。

这个注解可以添加到类定义或者 Configuration 类的 @Bean 方法，用来绑定并检验外部属性，例如.properties 文件。

注意：和@Value相反， SpEL 表达式不会被评估，因为属性值是外部化(externalized)的。

## 使用方式

最典型的使用方式，提供 prefix 和 locations 两个属性指定属性文件的来源和属性的前缀：

```java
@ConfigurationProperties(prefix = "dolphin", locations = "classpath:dolphin.properties")
public class DolphinServerProperties {}
```

## 参数列表

### 属性前缀

- value
- prefix

	要绑定到这个对象的有效的属性的名字前缀。value 和 prefix 是同义词

### 属性文件地址

- locations

	可选，提供清晰的资源位置用于绑定。默认这些指定位置的配置将和默认配置合并。这些资源优先于任何其他定义在环境中的属性来源。

- merge

	标记从指定位置转载的配置应该和默认配置合并。默认true。

### 对属性的检验

- ignoreInvalidFields

	标记当有非法字段绑定到这个对象时可以被忽略。"非法"指根据使用的binder非法，通常这意味着字段类型错误(或者无法强制转为正确的类型).

	默认false。这意味着不能忽略字段非法，所以只能报错。

- ignoreNestedProperties

	标记当绑定到这个对象字段的名字中带有句号(即".")时应该被忽略。默认false。

	默认false就不支持内嵌属性了，如果要支持需要设置为true。

- ignoreUnknownFields

	标记当有未知字段绑定到这个对象时可以被忽略。未知字段可能是属性中的一个符号错误。

	默认true。

- exceptionIfInvalid

	标记如果有可用的Validator并且检验失败时应该抛出异常。如果设置为false，验证错误将被吞掉。可能写日志，但是不传播到调用者。

    默认true。






