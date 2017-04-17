# part 2

在最后一篇文章中，我试图描述 Spring Boot starter 的内部工作。现在是开发自己的时候了！

作为一个例子，我们将使用 XStream，Thoughtworks 提供的一个 `no-fluff just-stuff` 的XML / JSON 序列化/反序列化器。建议只使用 JAXB 和 Jackson 的读者看看 XStream，它的效率非常高，其 API 非常易于使用。

如上一篇文章所示，starter 的起点在 `META-INF/spring.factories` 文件中。 让我们创建这样一个文件，带有适当内容：

```bash
org.springframework.boot.autoconfigure.EnableAutoConfiguration=ch.frankel.blog.xstream.XStreamAutoConfiguration
```

现在，我们来创建上面引用的类。如前所述，自动配置类只是一个常规的配置类。可以暂时保持为空。

```bash
@Configuration
public class XStreamAutoConfiguration {}
```

XStream 是围绕恰当命名的 `XStream` 类构建的，它是其序列化功能的入口。 Thoughtworks 设计用来避免使用静态方法，因此您需要一个 XStream 实例。 创建实例是无聊的重复任务，没有任何价值：它看起来像是 Spring bean 的完美目标。让我们在 auto-configuration 类中创建一个单例 bean，以便客户端应用程序可以使用它。我们的配置类变成：

```bash
@Configuration
public class XStreamAutoConfiguration {

    @Bean
    public XStream xstream() {
        return new XStream();
    }
}
```

XStream 网站上记录的 XStream no-args构造函数还有其他选择。例如 StaX，还有 JSON 等。我们的 starter 应该允许客户端应用程序使用自己的实例，因此只有在上下文中没有提供的情况下创建它。这听起来很像一个缺少bean的条件：

```bash
@Configuration
public class XStreamAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean(XStream.class)
    public XStream xstream() {
        return new XStream();
    }
}
```

XStream是基于转换器/converter的，转换器是一种从类型值转换为 JSON / XML 格式的字符串（和相反方向）的方式。 有很多开箱即用的预注册转换器，但客户可以注册自己的转换器。在这种情况下，应该可以在上下文中提供它们，以便它们用提供的实例进行注册。

```bash
@Bean
@ConditionalOnBean(Converter.class)
public Collection<Converter> converters(XStream xstream, Collection<Converter> converters) {
    converters.forEach(xstream::registerConverter);
    return converters;
}
```

此时，客户端应用程序在 Spring 上下文中提供的任何自定义转换器都将在 XStream 实例中注册。

总结关于创建 Spring Boot starter 。这很简单直接！在自己动手之前，不要忘了检查现有的 starter ，社区已经提供有很多开箱即用的。

更新：代码在 [Github](https://github.com/nfrankel/spring-boot-starter-xstream) 上。
