# RocketMQ

+ [下载安装](https://rocketmq.apache.org/docs/quickStart/01quickstart)

+ 启动：
```shell
$ nohup sh bin/mqnamesrv &

$ tail -f ~/logs/rocketmqlogs/namesrv.log
```
**The Name Server boot success...**

+ 启动Broker和Proxy

```shell
nohup sh bin/mqbroker -n localhost:9876 --enable-proxy &

tail -f ~/logs/rocketmqlogs/proxy.log 
```
The broker[broker-a, 10.0.3.31:10911] boot success. 

rocketmq-proxy startup successfully

一旦在proxy.log中看到“代理[代理名称，IP:端口]启动成功..”的信息，意味着**Broker已成功启动**。

**注意:**

已经成功部署了一个单主节点的RocketMQ集群，并且可以通过脚本发送和接收简单的消息。

## 测试
测试前将Nameserver地址设置到系统中，系统环境变量**NAMESRV_ADDR**。

```shell
export NAMESRV_ADDR=localhost:9876
```
注意：外网ip

**生产消息**

```shell
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
```

**接收消息**
```shell
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```

## Java应用

**集成**
```xml
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client-java</artifactId>
    <version>5.0.5</version>
</dependency>
```
**创建topic**

创建一个**TestTopic**
```shell
sh bin/mqadmin updatetopic -n localhost:9876 -t TestTopic -c DefaultCluster
```
注意：外网ip

## 停止

```shell
bin/mqshutdown broker

sh bin/mqshutdown namesrv
```