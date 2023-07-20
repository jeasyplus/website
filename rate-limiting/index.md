# 服务保护


![原理图](https://jeasyplus.com/rate-limiting/limit.png)
 T 表示 水管内部的水量，RT表示请求的处理时间，P表示请求数。

一个请求从进入水管到最终流出，水管中会存在 P * RT　个请求。

**T ≈ QPS * Avg(RT)**

系统处理能力和允许进入的请求数达到平衡。

## 目标：

+ **流量控制：** 针对突发的流量进行限制，尽可能处理请求的同时保障服务不被打垮。
+ **熔断降级：** 微服务架构下的复杂调用链路，某一环不稳定就会层层级联，需要对不稳定的弱依赖服务进行熔断降级。

需要考虑的问题：

1、如何定义流控降级与容错的标准？

2、支撑这些标准的实现有哪些，能帮助我们解决哪些问题？

相关内容：

+ [系统自适应保护](http://sentinelguard.io/zh-cn/docs/system-adaptive-protection.html)

+ [稳定的微服务系统时不得不考虑的场景](https://help.aliyun.com/practice_detail/444294)

## OpenSergo

开放通用的，覆盖微服务及上下游关联组件的微服务治理项目。

从**微服务**的角度出发，涵盖**流量治理、服务容错、服务元信息治理、安全治理**等关键治理领域。

最大特点就是**以统一的一套配置/DSL/协议定义服务治理规则，面向多语言异构化架构，覆盖微服务框架及上下游关联组件**。

## Sentinel

解决分布式系统中的服务保护和流量控制问题，提高服务的稳定性和可靠性。

### 流量控制规则管理：

管理员可以通过控制台或 API 配置流量控制规则，包括限流规则、熔断规则、系统负载保护规则等。

规则管理组件将这些配置信息存储在内存中，并根据配置来判断是否需要对请求进行限流或熔断。


### 实时监控和统计：

收集每个服务的请求情况、成功率、响应时间等信息，并提供实时监控面板和统计报告。

帮助运维人员及时发现服务的异常和瓶颈，并做出相应的调整。

### 熔断降级机制：

当服务出现异常或响应变慢时，熔断降级机制将自动触发熔断策略，避免故障服务的影响传递到其他服务，并提供友好的响应提示。

一旦服务恢复正常，熔断机制也会逐渐恢复服务的请求。


### Hystrix

```java

@HystrixCommand(fallbackMethod = "fallbackMethodAbc",
commandKey = "abc",
groupKey = "abc",
threadPoolKey = "abc",
threadPoolProperties = {
        @HystrixProperty(name = HystrixPropertiesManager.CORE_SIZE, value = "50"),
        @HystrixProperty(name = HystrixPropertiesManager.MAX_QUEUE_SIZE, value = "-1"),
},
commandProperties = {
        @HystrixProperty(name = HystrixPropertiesManager.EXECUTION_ISOLATION_THREAD_TIMEOUT_IN_MILLISECONDS, value = "3000"),
        @HystrixProperty(name = HystrixPropertiesManager.CIRCUIT_BREAKER_ERROR_THRESHOLD_PERCENTAGE, value = "50"),
        @HystrixProperty(name = HystrixPropertiesManager.CIRCUIT_BREAKER_REQUEST_VOLUME_THRESHOLD, value = "20"),
        @HystrixProperty(name = HystrixPropertiesManager.METRICS_ROLLING_STATS_TIME_IN_MILLISECONDS, value = "10000"),
})
```
**fallbackMethod = "fallbackMethodAbc"：**

指定断路器命令执行失败时调用的降级方法 fallbackMethodAbc。


**commandKey = "abc"：**

指定断路器命令的命令名称。



**groupKey = "abc"：**

指定断路器命令的组名称，将具有相同组名称的断路器命令归为同一组，用于共享一组线程池。



**threadPoolKey = "abc"：**

指定断路器命令使用的线程池的名称。



**threadPoolProperties：**

用于设置断路器命令使用的线程池的属性。在这里，设置了线程池的核心线程数为 50，最大队列大小为 -1（表示无限制）。



**commandProperties：** 用于设置断路器命令的属性。

**EXECUTION_ISOLATION_THREAD_TIMEOUT_IN_MILLISECONDS：**

指定断路器命令的超时时间为 3000 毫秒，即命令的执行时间超过 3000 毫秒则视为超时。

**CIRCUIT_BREAKER_ERROR_THRESHOLD_PERCENTAGE：**

指定断路器打开的错误阈值百分比为 50%，即在请求失败率达到 50% 时，断路器将被打开。
**CIRCUIT_BREAKER_REQUEST_VOLUME_THRESHOLD：**

（指定断路器的请求阈值为 20，即在一段时间内请求数量达到 20 个时，断路器将开始监测请求并采取相应的动作。

**METRICS_ROLLING_STATS_TIME_IN_MILLISECONDS：**

指定度量滚动统计的时间间隔为 10000 毫秒，即度量数据将按照 10 秒的间隔进行滚动统计和聚合。








