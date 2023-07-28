# shardingsphere

## ShardingSphere-JDBC

### 集成配置

#### maven
```xml
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId>shardingsphere-jdbc-core</artifactId>
    <version>5.4.0</version>
</dependency>
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId>shardingsphere-cluster-mode-repository-api</artifactId>
    <version>5.4.0</version>
</dependency>
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId>shardingsphere-cluster-mode-repository-nacos</artifactId>
    <version>5.4.0</version>
</dependency>

<dependency>
    <groupId>org.yaml</groupId>
    <artifactId>snakeyaml</artifactId>
    <version>1.33</version>
</dependency>
```

如果在项目中使用配置文件定义数据源和分片规则，就不用自定义Provider，如下：

**application.properties**
```properties
spring.datasource.url=jdbc:shardingsphere:classpath:server.yaml
spring.datasource.driver-class-name=org.apache.shardingsphere.driver.ShardingSphereDriver
```

如果使用nacos配置中心，需要自定义，如下
```properties
spring.datasource.url=jdbc:shardingsphere:nacos:127.0.0.1:8848:sharding
spring.datasource.driver-class-name=org.apache.shardingsphere.driver.ShardingSphereDriver
```

#### 自定义Provider
在resources目录下添加META-INF目录，新增一个SPI文件

**src/main/resources/META-INF/services/org.apache.shardingsphere.driver.jdbc.core.driver.ShardingSphereDriverURLProvider**

SPI文件内容：

```text
com.jeasyplus.shareding.NacosDriverURLProvider
```
**NacosDriverURLProvider.java**

```java

package com.jeasyplus.shareding;


import com.alibaba.nacos.api.NacosFactory;
import com.alibaba.nacos.api.config.ConfigService;
import com.alibaba.nacos.api.exception.NacosException;
import org.apache.commons.lang3.StringUtils;
import org.apache.shardingsphere.driver.jdbc.core.driver.ShardingSphereDriverURLProvider;

import java.util.Properties;

public final class NacosDriverURLProvider implements ShardingSphereDriverURLProvider {

    private final static String suffix = ":nacos:";

    private final static String DEFAULT_GROUP = "DEFAULT_GROUP";

    private final static String SPLIT_SYMBOL = ":";

    private final static long TIMEOUT = 5000;

    public NacosDriverURLProvider() {

    }

    public boolean accept(String url) {
        return StringUtils.isNotBlank(url) && url.contains(suffix);
    }

    public byte[] getContent(String url) {
        String data_group = url.substring(url.indexOf(suffix) + suffix.length());
        String serverAddr = null, dataId = "", port = "8848", group = DEFAULT_GROUP;
        if (data_group.contains(SPLIT_SYMBOL)) {
            String[] dataIdGroup = data_group.split(SPLIT_SYMBOL);
            serverAddr = dataIdGroup[0];
            if (dataIdGroup.length > 1) {
                port = dataIdGroup[1];
            }
            if (dataIdGroup.length > 2) {
                dataId = dataIdGroup[2];
            }
            if (dataIdGroup.length > 3) {
                group = dataIdGroup[3];
            }
        }
        serverAddr = serverAddr + ":" + port;
        Properties properties = new Properties();
        properties.put("serverAddr", serverAddr);
        //可以写到配置文件中，在上面代码中解析
        properties.put("username", "nacos");
        properties.put("password", "nacos");
        try {
            ConfigService configService = NacosFactory.createConfigService(properties);
            String config = configService.getConfig(dataId, group, TIMEOUT);
            return config.getBytes();
        } catch (NacosException e) {
            throw new RuntimeException(e);
        }
    }
}

```

### 数据库分片配置

共三个数据库实例，每个实例分拆两张表(**t_person_0、t_person_1**)。

附加一张从表，用于测试**绑定表**

**t_person**
```shell
create table jeasyplus.t_person_0
(
id         bigint auto_increment primary key,
first_name varchar(20) null,
last_name  varchar(20) null,
shared_id  int         null # 与id同值，如果全局使用一种分库策略，单独搞一个字段很有必要，否则，直接用业务字段
);
```
**t_person_tag**
```shell
create table jeasyplus.t_person_tag_0
(
    id        bigint auto_increment primary key,
    tag       varchar(12) null,
    person_id bigint         null,
    shared_id int         null # 与person_id同值
);
```

#### ShardingSphere-JDBC数据源和分片规则配置

[相关说明参考](https://shardingsphere.apache.org/document/current/cn/user-manual/shardingsphere-jdbc/yaml-config/rules/sharding/)

```yaml

databaseName: jeasyplus
dataSources:
  db_0:
    dataSourceClassName: com.zaxxer.hikari.HikariDataSource
    driverClassName: com.mysql.jdbc.Driver
    jdbcUrl: jdbc:mysql://127.0.0.1:3306/jeasyplus?useSSL=false&characterEncoding=utf8
    username: mysqldb
    password: mypw
  db_1:
    dataSourceClassName: com.zaxxer.hikari.HikariDataSource
    driverClassName: com.mysql.jdbc.Driver
    jdbcUrl: jdbc:mysql://127.0.0.1:3307/jeasyplus?useSSL=false&characterEncoding=utf8
    username: mysqldb2
    password: mypw
  db_2:
    dataSourceClassName: com.zaxxer.hikari.HikariDataSource
    driverClassName: com.mysql.jdbc.Driver
    jdbcUrl: jdbc:mysql://127.0.0.1:3308/jeasyplus?useSSL=false&characterEncoding=utf8
    username: mysqldb3
    password: mypw
props:
  sql:
    show: true
rules:
  - !SHARDING
    tables:
      t_person:      # 主表                                     
        actualDataNodes: db_${0..2}.t_person_${0..1}
        # databaseStrategy : # 分库策略，缺省表示使用默认分库策略，该示例统一使用默认策略，即下面配置的 database_inline。
        tableStrategy:
          standard:
            shardingColumn: id   #主表分片键
            shardingAlgorithmName: t_person_inline # 主表分片策略
      t_person_tag:   #从表
        actualDataNodes: db_${0..2}.t_person_tag_${0..1}
        tableStrategy:
          standard:
            shardingColumn: person_id  #从表分片键
            shardingAlgorithmName: t_person_tag_inline  #从表分片策略
      bindingTables:
        - t_person,t_person_tag  #绑定表，遇到联表查询数据集关联出错时再配
    defaultDatabaseStrategy:   
      standard:
        shardingColumn: shared_id # 数据库实例分片键
        shardingAlgorithmName: database_inline  # 数据库实例分片策略
    shardingAlgorithms:  #具体的分片策略（数据库、表），供上面引用
      database_inline:
        type: INLINE
        props:
          algorithm-expression: db_${shared_id % 3}
      t_person_inline:
        type: INLINE
        props:
          algorithm-expression: t_person_${id % 2}
      t_person_tag_inline:
        type: INLINE
        props:
          algorithm-expression: t_person_tag_${person_id % 2}
```

