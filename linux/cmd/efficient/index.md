# 高效工具

### dstat：

全能系统资源统计工具，用于实时监控系统性能。可以提供有关CPU使用率、内存使用率、磁盘I/O、网络传输、系统负载和进程状态等方面的详细统计信息。

dstat可以显示各种数据并支持多种输出格式，非常适合用于实时监控系统性能和快速诊断问题。

```shell
dstat -c -m -d -n -y -g --top-cpu --top-mem 1 5
```
每秒显示一次过去5秒内的CPU、内存、磁盘、网络和系统负载等方面的详细统计信息，并显示最消耗CPU和内存的进程。
```shell
alias dstat='dstat -cdlmnpsy'
```
-c: 显示CPU使用情况

-d: 显示磁盘I/O使用情况

-l: 显示磁盘和网络负载

-m: 显示内存使用情况

-n: 显示网络使用情况

-p: 显示进程使用情况

-s: 显示系统统计信息

-y: 显示文件系统使用情况

### htop:

交互式的系统监视工具，用于替代传统的top命令。可以显示系统中运行的进程列表，并实时更新进程的CPU、内存、带宽等使用情况。

使用htop可以更方便地查看和管理进程，以及监控系统资源的使用情况。

```shell
htop
```

## iotop:

用于监视磁盘IO的工具，可以实时显示磁盘IO的读写速率和进程的IO占用情况。

帮助找出哪些进程正在造成磁盘IO的高负载。

```shell
iotop
```


### iostat：
iostat代表"Input/Output Statistics"，用于监控设备（例如硬盘和分区）的输入/输出（I/O）统计信息。它提供有关磁盘读写操作、磁盘利用率和其他与I/O相关的指标的信息。

```shell
iostat -d 1 5
```
将每秒更换一次过去5秒内的磁盘I/O统计信息。

### vmstat：

vmstat代表"Virtual Memory Statistics"，提供有关系统各种统计信息的数据，包括CPU使用率、内存使用率和虚拟内存统计。它显示进程、分页、块I/O和CPU使用情况的数据。

```shell
vmstat 1 5
```
每秒显示一次过去5秒内的虚拟内存统计信息，包括CPU、内存、交换分区、进程和系统等方面的统计数据。

### ifstat：

ifstat代表"Interface Statistics"，用于监控网络接口的统计信息，例如每个网络接口的传入和传出数据包数、传输字节数以及网络利用率。

```shell
ifstat -i eth0 -t 1 5
```
每秒显示一次过去5秒内的eth0网络接口的传输统计信息。

### mtr

网络诊断工具，"Matt's Traceroute"的缩写，用于结合了ping和traceroute的功能。

通过连续发送数据包并显示数据包的往返时间（RTT）和丢包率来帮助诊断网络问题。

显示整个网络路径上的延迟、丢包和网络连接问题。

```shell
mtr www.example.com
```
### iftop

终端下的实时网络流量监控工具，它可以显示正在网络接口上发送和接收的数据流量。

默认按流量大小进行排序，并且可以显示源和目标IP地址、端口、流量速率、传输协议等信息。
```shell
sudo iftop -i eth0
```

### iptraf

实时网络流量监控工具，它提供了一个字符型界面和图形型界面供选择，支持多种网络接口的监控和数据包捕获。

可以显示各种网络统计信息，如流量速率、连接数、协议分布等。

允许您选择网络接口以监控其实时流量。

**图形界面**

```shell
sudo iptraf
```

**字符型界面**
```shell
sudo iptraf-ng
```


#### sar：

sar代表"System Activity Reporter"，是一个系统性能统计工具，用于收集和报告系统活动的历史数据。它可以收集系统的CPU使用率、内存使用率、磁盘I/O、网络活动等数据，并将这些数据记录在系统的日志文件中。通过查看sar生成的历史数据，管理员可以分析系统在过去的时间段内的性能表现和趋势，从而优化系统配置和资源分配。

```shell
sar -u 1 5
```

### netstat & lsof

```shell
##显示指定端口的信息。
sudo netstat -tuln | grep <port>

## 显示指定端口上的活动连接信息。
sudo lsof -i :<port>
```

### Siege

模拟并发用户访问网站或应用程序，以评估其性能和稳定性。

是一个命令行下的HTTP压力测试工具，它可以模拟多个并发用户访问网站，并测量网站的响应时间和吞吐量。

支持多种配置选项，可以设置并发用户数、请求时间、延迟等参数。

```shell
siege -c 100 -t 60s http://example.com
```
模拟100个并发用户在60秒内访问http://example.com，并显示测试结果。

### Tsung

一个开源的多协议性能测试工具，可以模拟大规模并发用户访问，并对网站或应用程序进行压力测试。

支持多种协议，如HTTP、WebSocket、SMTP、MySQL等，并提供了丰富的配置选项。

测试场景文件，如my_test.xml：
```xml
<tsung loglevel="notice">
  <clients>
    <client host="localhost" use_controller_vm="true"/>
  </clients>
  <servers>
    <server host="example.com" port="80" type="tcp"/>
  </servers>
  <load>
    <arrivalphase phase="1" duration="60" unit="second">
      <users interarrival="1" unit="second"/>
    </arrivalphase>
  </load>
  <sessions>
    <session probability="100" name="my_session" type="ts_http">
      <request>
        <http url="http://example.com" method="GET"/>
      </request>
    </session>
  </sessions>
</tsung>
```
启动测试：
```shell
tsung -f my_test.xml start
```




