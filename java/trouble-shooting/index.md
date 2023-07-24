# TroubleShooting

## 负载过高排查

```shell
top -c
```
M：以内存占用指标排序
P：以CPU指标排序

```shell
ps H -eo pid,tid,%cpu | grep 13323
```
ps: 显示当前运行的进程快照。
H: 显示进程的层次结构，包括进程及其子线程。
-eo pid,tid,%cpu: 指定显示的列，分别为进程号（PID）、线程号（TID）和CPU使用率（%CPU）。
-e: 显示所有进程，而不仅仅是当前用户的进程。
-o: 指定要显示的输出格式。可以使用逗号分隔的字段列表来定义输出的格式。例如，-o pid,tid,%cpu指定要显示进程号（PID）、线程号（TID）和CPU使用率（%CPU）这三个字段。
|: 管道符，将前面命令的输出作为后面命令的输入。
grep 13323: 在前面命令的输出中查找包含"13323"的行。

````shell
printf "%x\n" 15936 #线程号转16进制

[root@i6elmtmjcgljsliw ~]# printf "%x\n" 15936
3e40
````

```shell
jstack 15921|grep 3e40 -A20 #打印线程信息的前20行
```


## 扩展资源

[useful-scripts](https://github.com/oldratlee/useful-scripts)
