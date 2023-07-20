# Minikube

## 安装Docker
+ [安装Docker](https://jeasyplus.com/docker)

## 安装minikube

```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm

sudo rpm -Uvh minikube-latest.x86_64.rpm
```

#### 使用 docker 驱动程序启动集群：

```shell
minikube config set driver docker # 默认驱动程序

minikube delete # 删除之前其他配置

su docker # 切换到docker用户
minikube start # 启动

minikube start --driver=docker #或指定驱动程序启动集群
```


