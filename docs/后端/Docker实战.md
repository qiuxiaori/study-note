> 写在前面

- 操作系统：Windows10
- Docker版本：Docker version 19.03.5, build 633a0ea

### 一. 快速开始

**1. 安装：** [Docker官网](https://hub.docker.com/)，下载并安装

**2. 启动：** 安装后双击图标启动。

**3. 设置：** 右击docker图标，Settings -> Resources -> ADVANCED 调整Memory到4G，Swap到2G(根据电脑配置及实际需求调整)

### 二. 镜像

当运行容器时，使用的镜像如果在本地中不存在，docker 就会自动从 docker 镜像仓库中下载，默认是从 Docker Hub 公共镜像源下载。

**1. 管理和使用本地Docker主机镜像**

| 序号 | 命令 | 含义 | 示例 |
|:----|:----|:----|:----|
| 1 | [images/image ls] | 列出本机的镜像列表 | `docker images` |
| 2 | [pull] | 拉取docker镜像 | `docker pull node` |
| 3 | [search] | 搜索docker镜像源的镜像 | `docker search node` |
| 4 | [rmi] | 删除docker镜像 | `docker rmi node` |
| 5 | [tag] | 为镜像添加一个新的标签 | `docker tag 860c279d2fec node` |

**2. 构建docker镜像**

我们使用命令 docker build ， 从零开始来创建一个新的镜像。为此，我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

```Dockerfile
// 每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。
FROM    centos:6.7 // 指定使用哪个镜像源
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd // 告诉docker 在镜像内执行命令，安装了什么
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D // 要执行的命令
```

然后，我们使用 Dockerfile 文件，通过 docker build 命令来构建一个镜像。

```
docker build -t runoob/centos:6.7 .
// 参数说明
// -t ：指定要创建的目标镜像名
// . ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径
// --no-cache: 不使用缓存
```

### 三. 容器


| 序号 | 命令 | 含义 | 示例 |
|:----|:----|:----|:----|
| 1 | [run] | 启动容器 | ` docker run -it ubuntu /bin/bash` |
| 2 | [start] | 启动已停止运行的容器 |`docker start node` |
| 3 | [ps] | 查看已启动的容器 [-a] 所有容器 |`docker start node` |
| 4 | [stop] | 停止已启动的容器 |`docker stop id|name` |
| 5 | [restart] | 重启已启动的容器 | `docker restart id|name` |
| 6 | [attach] | 进入容器终端,退出时容器会停止 | `docker exec -it id|name /bin/bash` |
| 7 | [exec][-it] | 进入容器终端,退出时不会停止 | `docker exec -it id|name /bin/bash` |
| 8 | [rm][-f] | 删除容器 | `docker rm -f id|name` |
| 9 | [logs][-f] | 查看容器日志 [--tail][n] 查看最后n条日志 | `docker rm -f id|name` |

参数
 [-i]: 交互式操作，[-t]: 终端 [-d]: 后台运行 [-p]: 端口映射 --name 重命名

### 四. Docker仓库管理
> 仓库（Repository）是集中存放镜像的地方。目前 Docker 官方维护了一个公共仓库 [Docker Hub](https://hub.docker.com/)。大部分需求都可以通过在 Docker Hub 中直接下载镜像来实现。

**1. 登录**

登录需要输入用户名和密码，登录成功后，我们就可以从 docker hub 上拉取自己账号下的全部镜像。

`docker login`

**2. 退出**

退出 docker hub 可以使用命令：`docker logout`

**3. 推送镜像**

用户登录后，可以通过 docker push 命令将自己的镜像推送到 Docker Hub。
以下命令中的 username 替换为自己的 Docker 账号用户名。

```
$ docker tag ubuntu:18.04 username/ubuntu:18.04  // 将本地镜像加上自己的docker账号前缀
$ docker image ls

REPOSITORY      TAG        IMAGE ID            CREATED           ...  
ubuntu          18.04      275d79972a86        6 days ago        ...  
username/ubuntu 18.04      275d79972a86        6 days ago        ...    // 新镜像

$ docker push username/ubuntu:18.04 // 推送镜像到远程仓库
$ docker search username/ubuntu // 可以搜索到账号下的ubuntu镜像

NAME             DESCRIPTION       STARS         OFFICIAL    AUTOMATED
username/ubuntu
```

### 五. 实战：使用docker部署一个node项目

egg框架
去掉 --daemon
**1. 打包镜像**

```
FROM node:12

RUN mkdir -p /usr/src/node-app/asmr-os

WORKDIR /usr/src/node-app/asmr-os

COPY package.json /usr/src/node-app/asmr-os/package.json

RUN npm install

COPY . /usr/src/node-app/asmr-os

EXPOSE 7001

CMD ["npm", "start"]
```

`docker built -t asmr-os .`

**2. 运行项目**

```docker run -d --name asmr-os -p 7001:7001 asmr-os```

**3. 其他依赖**

### 六. 其他

**1. 查看构建过程：** `docker history [imageName]`
**2. 查看所有容器：** `docker ps -aq`
**3. 停止所有容器：** `docker stop $(docker ps -aq)`
**4. 删除所有容器：** `docker rm $(docker ps -aq)`
**5. 删除所有镜像：** `docker rmi $(docker images -q)`

### 七. Docker-compose

