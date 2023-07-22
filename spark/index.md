# Spark

+ [下载](https://spark.apache.org/downloads.html)

```shell
vim ~/.bash_profile
```
```shell
SPARK_HOME=/usr/local/spark-3.4.1

JAVA_HOME=/usr/lib/jvm/jdk-1.8-oracle-x64

$SPARK_HOME/bin
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


启动
```shell
su haddop

$SPARK_HOME/sbin/start-master.sh
```

创建一个job
```java
 public static void main(String[] args) {
        // 创建 Spark 配置对象
        SparkConf conf = new SparkConf().setAppName("SparkApp").setMaster("local[*]");

        // 创建 JavaSparkContext 对象
        JavaSparkContext sc = new JavaSparkContext(conf);

        // 创建一个包含数字的 JavaRDD
        JavaRDD<Integer> numbersRDD = sc.parallelize(Arrays.asList(1, 2, 3, 4, 5,13,232,2134,3240,34932,34234,324,3));

        // 使用 Spark 的算子计算平均值
        double average = numbersRDD.mapToDouble(num -> num).mean();

        // 打印结果
        System.out.println("Average: " + average);

        // 关闭 SparkContext
        sc.close();
}
```
pom.xml
```xml
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.12</artifactId>
    <version>3.4.1</version>
</dependency>

<build>
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
    </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.2.2</version>
        <configuration>
            <archive>
                <manifest>
                    <mainClass>org.example.SparkApp</mainClass>
                </manifest>
            </archive>
        </configuration>
    </plugin>
</plugins>
</build>
```
submit shell
```shell
vim run_spark_app.sh
```
```shell
SPARK_HOME=/usr/local/spark-3.4.1
JAR_FILE=/home/hadoop/sparkdemo-1.0-SNAPSHOT.jar

$SPARK_HOME/bin/spark-submit \
  --class org.example.SparkApp \
  --master yarn \
  --deploy-mode client \
  --executor-memory 2g \
  $JAR_FILE
```

## 其他操作
```shell
spark-shell.sh
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

http://localhost:4040

