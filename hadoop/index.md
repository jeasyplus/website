# Hadoop

+ [下载](https://www.apache.org/dyn/closer.cgi/hadoop/common/)

### 设置JAVA_HOME

```shell
which java

ls -l 


vim etc/hadoop/hadoop-env.sh

export JAVA_HOME=/usr/java/latest

source etc/hadoop/hadoop-env.sh
```
### 测试

```shell
bin/hadoop
```

### 单机模式

```shell
mkdir input

cp etc/hadoop/*.xml input

bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep input output 'dfs[a-z.]+'

cat output/*
```
### 伪分布式模式

etc/hadoop/core-site.xml:

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
etc/hadoop/hdfs-site.xml:
```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/tmp/hadoop-hduser/dfs/name</value>
    </property>
</configuration>
```

### 生成hadoop用户
```shell
sudo useradd -m hduser
sudo passwd hduser #hd1234qwer


sudo usermod -aG wheel hduser
# 或者
sudo usermod -aG sudo hduser    # 将hduser用户加入sudo组

sudo chown -R hduser:hduser /usr/local/hadoop-3.3.6/logs #赋予hduser日志目录权限

```
配置Hadoop用户环境变量：
```shell
vim etc/hadoop/hadoop-env.sh
```
```xml
export HDFS_NAMENODE_USER=hduser
export HDFS_DATANODE_USER=hduser
export HDFS_SECONDARYNAMENODE_USER=hduser
```

## 设置无密码 SSH（也称为无需密码的 SSH）

### 生成密钥对

```shell
sudo su hduser # 切换到 hduser 用户

ssh-keygen -t rsa #生成 SSH 密钥对

ssh-copy-id username@remote_server # 将公钥复制到远程服务器

ssh-copy-id hduser@localhost
# 或者
cat ~/.ssh/id_rsa.pub | ssh username@remote_server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

```


#### 测试无密码 SSH

在任意节点下都可以执行以下命令
```shell
ssh hduser@remote_server
```
否则就执行
```shell
ssh-copy-id hduser@remote_server
```


## 格式化namenode
```shell
bin/hdfs namenode -format #格式化namenode
```

## 启动
```shell
sudo su hduser

cd /usr/local/hadoop-3.3.6/

sbin/start-dfs.sh
```

为MapReduce任务创建HDFS目录

```shell
su hduser

bin/hdfs dfs -mkdir -p /user/hduser
```


copy文件到分布式文件系统
```shell
bin/hdfs dfs -mkdir input

bin/hdfs dfs -put etc/hadoop/*.xml input
```

运行example
```shell
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep input output 'dfs[a-z.]+'
```

查看结果
```shell
bin/hdfs dfs -get output output
cat output/*
```

### 查看日志

```shell
tail -f logs/hadoop-hduser-datanode-*.novalocal.log

tail -f ./logs/hadoop-hduser-namenode-*.novalocal.log
```

```shell
sbin/stop-dfs.sh # 停止
```

### 修改内存大小

vim etc/hadoop/hadoop-env.sh
```shell
export HDFS_NAMENODE_OPTS="-Xmx1g -Xms512m"
export HDFS_DATANODE_OPTS="-Xmx1g -Xms512m"
```

## YARN

etc/hadoop/mapred-site.xml:

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>
```

etc/hadoop/yarn-site.xml:

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```

### 添加环境变量

vim ~/.bash_profile

```shell
export JAVA_HOME=/usr/lib/jvm/jdk-1.8-oracle-x64/bin/java
export HADOOP_CONF_DIR=/usr/local/hadoop-3.3.6/etc/hadoop
export YARN_RESOURCEMANAGER_USER=hduser
export YARN_NODEMANAGER_USER=hduser
```

### 启动

```shell
 sbin/start-dfs.sh
```

停机
```shell
sbin/stop-dfs.sh
```