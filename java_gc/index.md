# jvm gc信息查看
## 查看系统信息：
```shell
top

top -Hp pid #查看当前进程下的线程信息
```

**查Java进程:**
```shell
jps
```
**查询线程:**
```shell
top -Hp java进程
jstack java进程
```

## arthas
https://arthas.aliyun.com/doc/install-detail.html
```shell
java -jar arthas-boot.jar

[arthas@2113]$ dashboard # 展示当前进程信息

[arthas@2113]$ jad #反编译

[arthas@2113]$ trace 类名 方法 ##跟踪方法的调用时间

[arthas@2113]$ redefine 类文件路径+类名.class
```

**打开gc日志**

```shell
vmoption PrintGC true

vmoption PrintGCDetails true
#等待应用程序的日志文件中输出gc信息
vmtool --action forceGc  #强制gc

vmoption PrintGCDateStamps true #为GC日志加个时间戳
```

开启内存对比信息
```shell
vmoption HeapDumpBeforeFullGC true

vmoption HeapDumpAfterFullGC  true

##GC日志输出
023-07-01T13:21:27.589-0800: [Heap Dump (after full gc): Dumping heap to java_pid5667.hprof.1 ...
Heap dump file created [39592661 bytes in 0.147 secs]
, 0.1469123 secs]

# 使用pwd查看当前应用的工作目录，
# 去对应目录查看.hprof文件
```

**dump分析工具**

IntelliJ IDEA plugins：**VisualVM Launcher**

**[visualvm](https://visualvm.github.io/download.html)**

**OQL**
```shell
select s from java.lang.String s where s.toString().startsWith("GH")

select * from java.lang.String  o where o.toString().starWith("324)
```

**查看内存**
```shell
memory
```
**查看线程信息**
```shell
thread

thread 线程id

thread -n 3 #查看最忙的前3个线程

[arthas@5667]$ thread -b #查询阻塞的线程
No most blocking thread found!

arthas thread -i 200 #每200毫秒动态刷新线程状态
```
jvm信息
```shell
jvm
```
输出内存信息
```shell
heapdump


[arthas@2113]$ heapdump /temp/dump.hpro
```
查看内存信息
```shell
jhat dump.hprof #Jdk jhat命令，而非arthas命令

jmc dump.hprof #如果jdk支持jmc，可以使用该命令
```

日志信息
```shell
logger

[arthas@2113]$ logger
 name                                ROOT                                                                                                                                                                             
 class                               ch.qos.logback.classic.Logger                                                                                                                                                    
 classLoader                         sun.misc.Launcher$AppClassLoader@18b4aac2                                                                                                                                        
 classLoaderHash                     18b4aac2                                                                                                                                                                         
 level                               INFO                                                                                                                                                                             
 effectiveLevel                      INFO                                                                                                                                                                             
 additivity                          true                                                                                                                                                                             
 codeSource                          file:/Users/nick/.m2/repository/ch/qos/logback/logback-classic/1.2.11/logback-classic-1.2.11.jar                                                                                 
 appenders                           name            CONSOLE                                                                                                                                                          
                                     class           ch.qos.logback.core.ConsoleAppender                                                                                                                              
                                     classLoader     sun.misc.Launcher$AppClassLoader@18b4aac2                                                                                                                        
                                     classLoaderHash 18b4aac2                                                                                                                                                         
                                     target          System.out                          
```
**追踪方法**

```java
watch com.example.demo.Tst hello
```


logger
**搜索已加载到JVM中的Class信息**
```shell
sc com.example.demo.DemoApplication
```

**搜索已加载的Class的方法信息**
```shell
sm com.example.demo.DemoApplication
```


**编译Java文件**
```shell
mc /temp/project/demo/src/main/java/com/example/demo/DemoApplication.java
```

**加载指定的class文件**
```shell
retransform /temp/project/target/classes/com/example/demo/DemoApplication.class
```
**查看ClassLoader**
```shell
classloader
```
**查看静态属性**

```shell
getstatic

[arthas@4174]$ getstatic com.example.demo.DemoApplication name
field: name
@String[Demo App]
Affect(row-cnt:1) cost in 25 ms.
```
```shell
ognl @com.example.demo.DemoApplication@name #使用ognl查看
@String[Demo App]
```

**查看服务的环境变量**

```shell
sysprop

sysprop | grep abc #查询环境变量abc的值
```

**输出到文件**
```shell
tee 

grep abc | tee /Users/nick/tools/abc.txt
```
**查看服务的工作目录**
```shell
pwd
```
**清理屏幕内容**

```shell
cls
```

**查看内容**

```shell
cat
```

**base64**

```shell
base64 /tmp/test.txt #编码

base64 --input /tmp/test.txt --output /tmp/result.txt #编码

base64 -d /tmp/result.txt #解码

base64 -d /tmp/result.txt --output /tmp/bbb.txt #解码
```

**动态开启Java Flight Recorder (JFR) **

```shell
jfr cmd status
 
#JDK8 8u262版本之后支持jfr

#$ jfr cmd [start，status，dump，stop]
#$ jfr start -n myRecording --duration 60s -f /tmp/myRecording.jfr
# 指定记录名，记录持续时间，记录文件保存路径。
```



