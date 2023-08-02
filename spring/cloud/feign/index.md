# Feign

pom.xml
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-loadbalancer</artifactId>
</dependency>
```
config.java
```java
@EnableFeignClients
```
provider.java
```java
@RestController
@RequestMapping("person")
public class MainController {

    @Autowired
    private PersonRepo repo;

    @RequestMapping("list")
    public List list() {
        return repo.findAll();
    }
}
```
client.java
```java
@FeignClient("producer")
public interface StoreClient {

    @RequestMapping(method = RequestMethod.GET, value = "/person/list")
    List<PersonEntity> list();

}
```
application.yaml
```yaml
spring:
    loadbalancer:
        nacos:
            enabled: true
        ribbon: 
            enabled: false
```