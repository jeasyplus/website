# 使用Docker安装MySQL


## 环境准备


+ [安装](https://jeasyplus.com/docker)
+ 检查并启动docker

```text

sudo systemctl status docker

sudo systemctl start docker
```

## 下载MySQL镜像

[MySQL 企业版映像]https://container-registry.oracle.com/

社区版
```shell
docker pull container-registry.oracle.com/mysql/community-server:tag #tag 指定版本，如：5.78.0latestlatest


docker pull container-registry.oracle.com/mysql/community-server # 下载最新版

docker images # 查看镜像
```

## 启动


```shell
docker run --name=container_name  --restart on-failure -d image_name:tag

docker run --name=mysql-slave2 --restart on-failure  \
-d -p 3307:3306 container-registry.oracle.com/mysql/community-server:latest

docker run --name=mysql-slave2 --restart on-failure  \
   -v /var/lib/mysql-slave2:/var/lib/mysql \
-d -p 3307:3306 container-registry.oracle.com/mysql/community-server:latest  #指定数据卷目录

docker ps # 查看运行的镜像

docker logs mysql-slave2 #查看日志
```

## 修改密码

```shell
docker logs mysql-slave2 2>&1 | grep GENERATED


docker exec -it mysql-slave2 mysql -uroot -p

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';
```

## 查看数据卷
```shell
docker inspect --format='{{json .Mounts}}' container_id
```

## 登录

```shell
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id

mysql -h container_ip -P 3307 -u username -p
```


## 异常

```text
docker: Error response from daemon: driver failed programming external connectivity on endpoint mysql-slave3 (00de4803c92f2049429b33e8adb4d1acfa8717d6b85bb750b63889ca27630200):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 3308 -j DNAT --to-destination 172.17.0.2:3308 ! -i docker0: iptables: No chain/target/match by that name.
```
解决方法

````shell
sudo systemctl restart docker

sudo iptables -t nat -F
sudo iptables -t filter -F
sudo iptables -t mangle -F
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
````

