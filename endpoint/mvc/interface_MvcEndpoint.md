# 接口MvcEndpoint

在端点之上的MVC层策略。

实现类允许使用 `@RequestMapping` 和完整的 `Spring MVC` 机制，但是不能在类型级别使用`@Controller` 或 `@RequestMapping`，因为这将导致路径的双重映射： 一次通过常规MVC处理程序映射，一次通过 EndpointHandlerMapping。

## 接口定义

```java
package org.springframework.boot.actuate.endpoint.mvc;

public interface MvcEndpoint {}
```


## 接口方法

- getPath()

	返回端点的 MVC 路径.

	```java
    String getPath();
    ```

	备注：类似于 Endpoint 的 getID()方法。

- isSensitive()

	返回端点是否敏感。

	```java
    boolean isSensitive();
    ```

- getEndpointType()

	返回暴露的 Endpoint 类型，或者返回 null，如果暴露的信息不能被表示为传统的 Endpoint。

	```java
   Class<? extends Endpoint> getEndpointType();
    ```

## 分析

对比 MvcEndpoint 和 Endpoint：

1. getPath() 方法类似 Endpoint 的 getID()方法。
2. 没有 isEnabled() 方法
3. 没有 invoke() 方法
4. 多了一个 getEndpointType()方法

和直觉不相符的是 MvcEndpoint 没有继承 Endpoint, 也就是说 MvcEndpoint 不是一个  Endpoint。再回顾下面这句关于 MvcEndpoint 定位的话：

**A strategy for the MVC layer on top of an  Endpoint**

再结合 getEndpointType() ，我们可以看到 MvcEndpoint 不是一个 Endpoint 的实现，而是一种暴露 Endpoint 的方式，详细的说是一种通过 Mvc 暴露 Endpoint 的方式。因此，MvcEndpoint 的准确名称应该是 EndpointExposureMvcImpl 。

