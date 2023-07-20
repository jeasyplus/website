# Docker

## 安装


Uninstall old versions
```shell
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
```
### Install using the rpm repository

#### Set up the repository
```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

##### listing the available versions in the repository:
```shell
yum list docker-ce --showduplicates | sort -r
```

##### To install the latest version, run:

```shell
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Start Docker

```shell
sudo systemctl start docker
```

```shell
sudo docker run hello-world
```

### 创建docker用户

```shell
sudo useradd -m -g docker docker # 创建用户并加入docker组

sudo passwd docker # 设置密码

id docker #验证

```
