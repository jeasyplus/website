# Spark

+ [下载](https://spark.apache.org/downloads.html)

```shell
vim ~/.bash_profile

SPARK_HOME=/usr/local/spark-3.4.1

JAVA_HOME=/usr/lib/jvm/jdk-1.8-oracle-x64

$SPARK_HOME/bin
```

执行
```shell
spark-shell
```
如果如下异常，添加hadoop classpath到 conf/spark-env.sh
```shell
# spark-shell
Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/hadoop/fs/FSDataInputStream
	at java.lang.Class.getDeclaredMethods0(Native Method)
	at java.lang.Class.privateGetDeclaredMethods(Class.java:2701)
	at java.lang.Class.privateGetMethodRecursive(Class.java:3048)
	at java.lang.Class.getMethod0(Class.java:3018)
	at java.lang.Class.getMethod(Class.java:1784)
	at sun.launcher.LauncherHelper.validateMainClass(LauncherHelper.java:669)
	at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:651)
Caused by: java.lang.ClassNotFoundException: org.apache.hadoop.fs.FSDataInputStream
	at java.net.URLClassLoader.findClass(URLClassLoader.java:387)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:355)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
	... 7 more
```

```shell
vim conf/spark-env.sh
```

```shell
export HADOOP_CONF_DIR=/usr/local/hadoop-3.3.6/etc/hadoop

export YARN_CONF_DIR=/usr/local/hadoop-3.3.6/etc/hadoop #, to point Spark towards YARN configuration files when you use YARN

export HADOOP_HOME=/usr/local/hadoop-3.3.6

export SPARK_DIST_CLASSPATH=$HADOOP_HOME/share/hadoop/common/*:$HADOOP_HOME/share/hadoop/common/lib/*:$HADOOP_HOME/share/hadoop/hdfs/*:$HADOOP_HOME/share/hadoop/hdfs/lib/*:$HADOOP_HOME/share/hadoop/mapreduce/*:$HADOOP_HOME/share/hadoop/mapreduce/lib/*:$HADOOP_HOME/share/hadoop/yarn/*:$HADOOP_HOME/share/hadoop/yarn/lib/*
```


http://localhost:4040

