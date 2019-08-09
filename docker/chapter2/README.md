# docker镜像

```
# 获取镜像
docker [image] pull NAME[:TAG]

# 列出镜像
docker images
docker image ls

# 添加镜像标签
docker tag ubuntu:latest myubuntu:latest

# 查看详细信息
docker [image] inspect NAME[:TAG]

# 查看镜像历史，列出镜像各层创建信息
docker history NAME[:TAG]

# 搜寻镜像
docker search [option] keyword

# 删除镜像
docker rmi IMAGE 
docker image rm IMAGE

# 清理临时或不用的镜像文件
docker image prune
```

## 一、创建镜像

### 1.1 基于已有容器创建

`docker [container] commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`

```
-a : 作者信息
-c ： 提价的时候执行Dockerfile指令
-m ： 提交消息
-p ： 提交时暂停容器运行
```

```
[vagrant@localhost ~]$ docker run -it ubuntu /bin/bash
root@822218bb2652:/# touch test
root@822218bb2652:/# exit
exit
[vagrant@localhost ~]$ docker commit -m "Added a new file" -a "Docker Newbee" 822218bb2652 test:0.1
sha256:a9c8eb4f717d4a85fa4e221bbbbef344ec2b3d4cbc1738f6ae3162480a82ff32
[vagrant@localhost ~]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test                0.1                 a9c8eb4f717d        5 seconds ago       64.2MB
ubuntu              latest              3556258649b2        2 weeks ago         64.2MB
myubuntu            latest              3556258649b2        2 weeks ago         64.2MB
[vagrant@localhost ~]$
```

### 1.2 基于本地模板导入

`docker [image] import [OPTIONS] file|URL|-[REPOSITORY[:TAG]]`

### 1.3 基于Dockerfile创建

Dockerfile是一个文本文件，利用给定的指令描述基于某个父镜像创建新镜像的过程

```

FROM debian:stretch-slim

LABEL version="1.0" maintainer="docker user <docker_user@github>"

RUN apt-get update && \
    apt-get install -y python3 && \
    apt-get clean && \
    rm -fr /var/lib/apt/lists/*
```

`docker [image] build -t python:3 .`

## 二、存出和载入镜像

```
# 存出镜像
docker save -o ubuntu_latest.tar ubuntu:latest

# 载入镜像
docker load -i ubuntu_latest.tar
docker load < ubuntu_latest.tar
```

## 三、上传镜像

`docker [image] push NAME[:TAG] | [REGISTRY_HOST[:REGISTRY_PORT]/]NAME[:TAG]`

