# Hive

+ [下载](https://www.apache.org/dyn/closer.cgi/hive)
+ [hive+spark版本匹配列表](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark%3A+Getting+Started)




## 安装


```shell
vim ~/.bash_profile
```
```shell
export HIVE_HOME=/usr/local/apache-hive-3.1.3-bin
PATH=$PATH:$HIVE_HOME/bin
```
````shell
 source ~/.bash_profile
````

vim conf/hive-env.sh
```shell
export JAVA_HOME=/usr/lib/jvm/jdk-1.8-oracle-x64
export HIVE_HOME=/usr/local/apache-hive-3.1.3-bin
export HADOOP_HOME=/usr/local/hadoop-3.3.6
export HIVE_CONF_DIR=/usr/local/apache-hive-3.1.3-bin/conf
export HIVE_AUX_JARS_PATH=/usr/local/apache-hive-3.1.3-bin/lib
```

## 修改配置（可选）
```shell
mv conf/hive-default.xml.template  conf/hive-site.xml

vim  vim  conf/hive-log4j2.properties.template conf/hive-log4j2.properties

property.hive.log.dir = /var/log/hive/${sys:user.name}
```

上传相应的jdbc Driver到$hive_home/lib下
配置mateStore
```shell
vim conf/hive-site.xml
```
```shell

<name>javax.jdo.option.ConnectionURL</name>
<value>jdbc:mysql://127.0.0.1:3306/hive?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false&amp;useAffectedRows=true</value>


<name>javax.jdo.option.ConnectionUserName</name>


<name>javax.jdo.option.ConnectionPassword</name>

<name>javax.jdo.option.ConnectionDriverName</name>
<value>com.mysql.cj.jdbc.Driver</value>
```

```shell
<name>hive.server2.logging.operation.log.location</name>
<value>/tmp/hadoop/operation_logs</value>

<name>hive.querylog.location</name>
<value>/tmp/hadoop</value>

<name>hive.exec.local.scratchdir</name>
<value>/tmp/hadoop</value>

<name>hive.downloaded.resources.dir</name>
<value>/tmp/hadoop_resources</value>
```

如果出现初始中出现如下异常，定位到指定行，删除该行或删除特殊字符
```shell
Exception in thread "main" java.lang.RuntimeException: com.ctc.wstx.exc.WstxParsingException: Illegal character entity: expansion character (code 0x8
at [row,col,system-id]: [3215,96,"file:/usr/local/apache-hive-3.1.3-bin/conf/hive-site.xml"]
```
```shell
vim +3215 conf/hive-site.xml
```

### 初始化数据
```shell
bin/schematool -initSchema -dbType mysql
```


## 运行

```shell
su hadoop -c hive

# 或者

su hadoop

hive

dfs -ls /; #查看hdfs文件系统

! ls /home/hadoop; 看本地文件系统
```

## 建数据库

```shell
create database if not exists jeasyplus;
```
```shell
show databases;

show databases like 'j*';

desc database jeasyplus; # 查看数据库信息

use jeasyplus; #切换数据库

alter database jeasyplus set dbproperties('createtime'='2023'); #设置DBPROPERTIES属性

desc database extended jeasyplus; 

drop database jeasyplus;
```

## 设置hive命令行
```shell
set hive.cli.print.current.db=true; #显示当前库

set hive.cli.print.header=true; # 

```
设置只对当前会话有效，可以创建一个文件保存 vim ～/.hiverc 。

## 建表

**内部表（Internal Table）**

```shell
CREATE TABLE IF NOT EXISTS employees (
  id INT,
  name STRING,
  age INT,
  department STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
```
+ 内部表是Hive默认的表类型。
+ 内部表的数据由Hive管理，当删除内部表时，Hive会同时删除表的数据。
+ 内部表适用于临时性的数据存储，或者数据仅在Hive中使用的情况。
+ 内部表的数据通常存储在Hive的数据仓库目录中。
+ 内部表对Hive的元数据管理较为方便，可以直接在Hive中查看和操作表的数据。

**外部表（External Table）**
```shell
CREATE EXTERNAL TABLE user_data (
  id BIGINT,
  user_id STRING,
  gmt_modified TIMESTAMP,
  gmt_create TIMESTAMP,
  pending_reward INT,
  description STRING
)
PARTITIONED BY (pt STRING)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
STORED AS TEXTFILE
LOCATION 'hdfs://localhost:9000/user/hive/warehouse/dwd_database.db/user_data';
```

+ 外部表的数据不由Hive管理，而是外部数据存储系统（如HDFS）管理。
+ 外部表适用于需要与外部数据源交互的情况，比如与其他系统共享数据。
+ 外部表的数据存储在指定的外部数据源（比如HDFS）中，并不在Hive的数据仓库目录下。
+ 当删除外部表时，Hive只会删除表的元数据，而不会删除实际的数据。这使得外部表在删除时更加谨慎，不会误删数据。
+ 外部表对于数据的格式转换和数据迁移非常有用。你可以在Hive中创建外部表来访问其他系统中的数据，而无需实际将数据复制到Hive的数据仓库目录中。

**外部表**与**普通表**（CREATE TABLE）有所不同。

**外部表**的数据**不存储在Hive的数据仓库目录**中，而是直接在指定的**HDFS路径**下。

在**删除外部表**时，**数据不会被自动清除**，只会删除表的元数据。

内部表适合管理和维护Hive中的数据，而外部表适用于与外部数据源进行交互、共享数据或进行数据格式转换的场景。

## 创建数据

创建hdfs目录
```shell
hdfs dfs -mkdir -p hdfs://localhost:9000/user/hive/warehouse/dwd_database.db/user_data
```
创建数据文件
```shell
vim user_data.txt
```
```text
1\tuser123\t2023-07-22 12:34:56\t2023-07-22 10:00:00\t100\tSample description 1
2\tuser456\t2023-07-22 15:45:30\t2023-07-22 11:30:00\t50\tSample description 2
3\tuser789\t2023-07-22 18:20:15\t2023-07-22 09:15:00\t200\tSample description 3
```
上传数据文件到hdfs

```shell
hdfs dfs -put ~/user_data.txt hdfs://localhost:9000/user/hive/warehouse/dwd_database.db/user_data/
```
加载数据到hive
```shell
hive

use jeasyplus;

LOAD DATA INPATH '/user/hive/warehouse/dwd_database.db/user_data/user_data.txt' INTO TABLE user_data;
```

### 查询
```shell
hive -e "select id from jeasyplus.user_data;" 

hive -f query.sql
```

## 更换执行引擎

```shell
 vim conf/hive-site.xml
```
```xml
<property>
<name>hive.execution.engine</name>
<value>spark</value>
</property>
```

集成spark后，执行如果出现spark相关class找不到的情况可以，把spark相关jar上传到hdfs上，并通过配置告之hive
```shell
hdfs dfs -put /usr/local/spark-3.3.2/jars/* /user/<your_username>/spark-jars/

hdfs dfs -ls /user/hadoop/spark-jars/
Found 3 items
-rw-r--r--   1 hadoop supergroup    5443542 2023-07-23 20:57 /user/hadoop/spark-jars/scala-library-2.12.15.jar
-rw-r--r--   1 hadoop supergroup   11008412 2023-07-23 20:31 /user/hadoop/spark-jars/spark-core_2.12-3.3.2.jar
-rw-r--r--   1 hadoop supergroup    2415854 2023-07-23 21:04 /user/hadoop/spark-jars/spark-network-common_2.12-3.3.2.jar
```

```shell
 vim conf/hive-site.xml
```
```shell
<property>
  <name>spark.yarn.jars</name>
  <value>hdfs://xxxx:8020/spark-jars/*</value>
</property>
```
hdfs单机版：端口为9000





