# Zipkin

+ [zipkin](https://zipkin.io/pages/quickstart)

pom.xml
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
```
applicatoin.yaml
```yaml
spring:
  zipkin:
    base-url: http://127.0.0.1:9411
```

## 自定义追踪

异常日志
```java
@ControllerAdvice
public class ExceptionHandler {

    @org.springframework.web.bind.annotation.ExceptionHandler(Exception.class)
    public Object handler(Exception ex) {
        Tracer tracer = ApplicationContextHolder.getBean(Tracer.class);
        Span log = tracer.nextSpan().name("app-exception-log").start();
        try (Tracer.SpanInScope ws = tracer.withSpan(log)) {
            // 执行自定义的业务逻辑
            StringWriter stringWriter = new StringWriter();
            ex.printStackTrace(new PrintWriter(stringWriter));
            tracer.currentSpan().tag("app-exception-log", stringWriter.toString());
        } finally {
            log.end();
        }
        throw new RuntimeException(ex);
    }
}
```