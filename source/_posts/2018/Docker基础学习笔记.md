---
title: Docker基础学习笔记
date: 2018-01-27 22:24:26
---


## Docker基本使用

`docker search`  搜索镜像

`docker pull REPOSITORY[:TAG]`  获取镜像

`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`  运行Docker容器。
（如果本地没有镜像会自动从仓库获取）

* `-d` 后台运行
* `-P` 容器端口随机映射到宿主机
* `-p 宿主机端口:容器端口` 端口绑定
* `-i` 启动一个可交互容器
* `-t` 使用虚拟终端关联到容器的标准输入输出
* `-v 宿主机目录:容器目录` 挂载数据卷
* `--rm` 使用后销毁

<!-- more -->

`docker ps`  查看Docker容器信息（默认只看正在运行的）
* `-a` 查看所有容器

**其它命令**

`docker port`  查看容器的端口映射情况

`docker logs`  查看容器的日志
* `-f`可以持续输出log信息

`docker top`   查看容器的进程

`docker inspect`  查看容器底层信息，返回一个json

`docker stop`  停止容器

`docker rm`  删除容器（需要先停止）

`docker rmi`  删除镜像

`docker images`  列出本地主机上的镜像

`docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`  提交对容器的修改，创建新镜像

`docker build`  构建镜像

`docker tag`  设置镜像标签

## Docker与虚拟机

很多人喜欢把Docker解释为轻量级虚拟机，这往往使人困惑。Docker容器是宿主机上的进程，是Docker Daemon的子进程，通过Namespace实现容器间的进程隔离。Namespace还有网络隔离、使容器有独立主机名，从而使得容器可视为独立节点。Docker容器利用chroot，形成了独立的运行环境。

## Dockerfile

`FROM`  指定基础镜像

`RUN`  执行命令

`COPY`  复制文件

`CMD`  指定默认的容器主进程的启动命令

`ENV`  设置环境变量

`VOLUME`  定义匿名数据卷

`EXPOSE`  声明端口

`WORKDIR`  指定工作目录

## Docker Compose

Docker Compose用于运行多个Docker容器，使用YAML文件作为配置文件，文件名一般是docker-compose.yml。
YAML文件使用缩进来表示层级关系，在编写时需要注意。

[Docker Compose详细配置](https://docs.docker.com/compose/compose-file/)

`docker-compose up`  用Compose启动应用

`docker-compose stop`  用Compose停止应用

`docker-compose down`  用Compose移除容器

* ` --volumes`  连同数据卷也移除

## Docker Machine

使用VirtualBox驱动创建Docker主机：`docker-machine create --driver virtualbox <machine-name>`

启动Docker主机：`docker-machine start <machine-name>`

停止Docker主机：`docker-machine stop <machine-name>`

SSH连接Docker主机：`docker-machine ssh <machine-name>`

在Docker主机中执行命令：`docker-machine ssh <machine-name> "<在Docker主机中执行的命令>"`

查看Docker主机：`docker-machine ls`

## swarm集群与负载均衡

初始化swarm集群管理节点：`docker swarm init --advertise-addr <ip>:<port>`（ ip 通过`docker-machine ls`得到, port 一般是2377）

加入集群，作为工作节点：`docker swarm join --token <token> <ip>:<port>`（token 在初始化管理节点时会显示）

查看集群中的节点：`docker node ls`

部署应用：`docker stack deploy -c docker-compose.yml <app-name>`

清除应用：`docker stack rm <app-name>`

离开swarm：`docker swarm leave`（管理节点离开swarm需要加上`-f`/`--force`参数以强制离开）

部署`dockersamples/visualizer:stable`服务可以在浏览器可视化集群。
