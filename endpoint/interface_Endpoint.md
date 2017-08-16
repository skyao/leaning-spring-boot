# 接口Endpoint

可用于将有用信息暴露给运维的端点。通常通过 Spring Mvc 暴露，但是也可以通过某些其他技术暴露。如果开发你自己的端点，可以考虑继承 AbstractEndpoint。

## 接口定义

```java
package org.springframework.boot.actuate.endpoint;

public interface Endpoint<T> {}
```

T 是端点数据类型。

## 接口方法

- getId()

	端点的逻辑ID。只能包含简单字母，数字和"_"字符。

	```java
    String getId();
    ```

- isEnabled()

	返回端点是否开启。

	```java
    boolean isEnabled();
    ```

- isSensitive()

	返回端点是否敏感。例如返回的数据是普通用户不应该看到的。

	```java
    boolean isSensitive();
    ```

- invoke()

	用来调用端点，返回端点调用的结果。

	```java
    T invoke();
    ```