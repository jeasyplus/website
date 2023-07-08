MySQL安装指南

**CentOS：**
下载MySQL Yum仓库信息
https://dev.mysql.com/downloads/repo/yum/

## 更新yum仓库信息
```shell
chmod +x mysql80-community-release-el7-7.noarch.rpm

rpm -Uvh mysql80-community-release-el7-7.noarch.rpm
```

**查看mysql yum信息**
```shell
yum repolist all | grep mysql #查看所有版本

yum repolist enabled | grep mysql #查看已启用的版本
```
**启用新版本**

如果启用的是期望的版本，跳过此步
如果没有yum-config-manager，直接修改yum配置
```shell
sudo yum-config-manager --disable mysql80-community #启用mysql8.0

sudo yum-config-manager --enable mysql57-community #启用mysql5.0

#或者直接修改配置

vim /etc/yum.repos.d/mysql-community.repo

#打开对应版本的开关
enabled=1
```

**关闭默认的模块**
```shell
sudo yum module disable mysql #执行不成功可以跳过
```

## 安装MySQL
```shell
sudo yum install mysql-community-server
```

## 启动 MySQL 服务器

```shell
systemctl start mysqld #启动

systemctl status mysqld #查看
```

**查看初始密码**
```shell
sudo grep 'temporary password' /var/log/mysqld.log

#冒号后面的符串即密码，
[Note] [MY-010454] [Server] A temporary password is generated for root@localhost: -evm_JGZX4eM
```

**登录mysql**
先复制密码到文本中，然后到登录
```shell
mysql -uroot -p 

#登录成功修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```

## 添加远程访问用户
如无必要，请不要添加

```roomsql

-- 添加用户
CREATE USER '<username>'@'<remote_host>' IDENTIFIED BY '<password>';
-- 或者
CREATE USER 'remote_user'@'%' IDENTIFIED BY 'password';


-- 授权数据库到用户
GRANT ALL PRIVILEGES ON <database_name>.* TO '<username>'@'<remote_host>';
-- 或者
GRANT ALL PRIVILEGES ON mydatabase.* TO 'remote_user'@'%';

-- 刷新MySQL权限以使更改生效
FLUSH PRIVILEGES;

```

