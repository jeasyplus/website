# linux目录

## home
当前用户的工作目录，登录后默认会进行到这个目录下。
```shell
/home/root
```

## dev
**device**--设备/装置（管理），**该目录位于内存中**。所以硬件参数都保存在这个目录下。如插入硬盘：
```shell
sda sdb sdc sdd #sd[a-z]多个硬盘
sda1 sda2 sda3 #sd[a-z][1~9]多个分区
```

## var
**variable** 变量，系统和程序运行时产生或用到的文件都会释放到这个目录下。如，日志文件、cache缓存。
```shell
/var/run #程序运行时的状态和进程编号
/var/lock #记录某些已经正在使用的文件
```
## proc
**process directory** 进程目录，**该目录位于内存中**。
```shell
ls /proc/123 #列出123进程的详细状态
ls /proc/environ #记录进程的环境变量
ls /proc/men #定义了进行占用大小
ls /proc/cmdline #定义了进程的启动命令
```
在进程没有启动时，对应的目录不会存在，关机重启会自动清除。

## usr
**unix software resource** 软件资源目录
```shell
/usr/bin #安装软件时产生的指令，zip、wget、vim
/usr/lib/ #非系统应用目录
/usr/local/ #软件安装目录
/usr/sbin/ #保存shell指令，只能被root执行
/usr/src #源文件
/usr/share/ #共享目录，文件、软说明...
/usr/include #非系统程序的头文件
```


## sbin
```shell
/bin #无需任何权限
/sbin #系统启动后要用
/sbin #（system bin）权限敏感的命令，如：shutdown、reboot、init、runlevel、insmod

/usr/bin #
/usr/sbin #应用程序启动后用
/usr/sbin #应用需要的指令，系统非必需的指令，如：ssh、vi、gcc。
```

## boot
**内核目录**，系统启动时BIOS读取的目录。
```shell
cat /boot/config-*-*-* #内核文件
efi #BIOS内核文件就会读取该文件，即访问系统分区
grub #访问完系统分区就会该文件，即引导系统启动，如：菜单的选项、超时时间、背景图片
grup2 #grub升级版
initramfs-*-*-*-*.img #提供一个基本的环境，处理一些常见问题，如硬件检测、设备挂载、加密锁等等
```

link files
```shell
ln -s [源目录] [目标目录] 创建软链接
ln [源目录] [目标目录] 创建硬链接
```














