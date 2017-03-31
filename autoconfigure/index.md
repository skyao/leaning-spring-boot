# AutoConfigure

Spring Boot可以根据 classpath 的内容配置大部分常见应用程序。单个 `@EnableAutoConfiguration` 注解触发 Spring context 的自动配置。

自动配置尝试推断用户可能需要哪些bean。例如，如果HSQLDB位于类路径上，并且用户尚未配置任何数据库连接，则可能需要定义内存数据库。随着用户开始定义自己的bean，自动配置将退出。


