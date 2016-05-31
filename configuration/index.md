配置
========

## 配置方式

### @Value 注解

使用@Value注解，可以直接将属性值注入到beans中。

> 注： 详细资料见 [24. Externalized Configuration](http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) 或者 中文翻译 [外化配置](https://qbgbook.gitbooks.io/spring-boot-reference-guide-zh/content/IV.%20Spring%20Boot%20features/23.%20Externalized%20Configuration.html)

```java
@Component
public class MyBean {
    @Value("${name}")
    private String name;
    // ...
}
```


### @ConfigurationProperties 注解

> 注： 详细资料见 [类型安全的配置属性](https://qbgbook.gitbooks.io/spring-boot-reference-guide-zh/content/IV.%20Spring%20Boot%20features/23.7.%20Typesafe%20Configuration%20Properties.html)

TBD

和 @EnableConfigurationProperties注解 配合使用

```java
@Service
public class MyService {
    @Autowired
    private ConnectionSettings connection;
     //...
    @PostConstruct
    public void openConnection() {
        Server server = new Server();
        this.connection.configure(server);
    }
}
```




