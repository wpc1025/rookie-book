# docker容器

容器是镜像的一个运行实例，镜像是静态的只读文件，容器带有运行时需要的可写文件层。容器中的应用进程处于运行状态。


## 一、创建容器

### 1. 新建容器

`docker [container] create`


```
[vagrant@localhost ~]$ docker container create ubuntu
3399e981d0b774547eb3a6555b2137e4fea0b78d3985e986ca62a2036383bfe5
[vagrant@localhost ~]$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
3399e981d0b7        ubuntu              "/bin/bash"         5 seconds ago       Created                                 relaxed_newton
[vagrant@localhost ~]$
```

### 2. 启动容器

`docker [container] start`

```
[vagrant@localhost ~]$ docker start 3399e981d0b7
3399e981d0b7
[vagrant@localhost ~]$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                     PORTS               NAMES
3399e981d0b7        ubuntu              "/bin/bash"         About a minute ago   Exited (0) 2 seconds ago                       relaxed_newton
[vagrant@localhost ~]$
```

### 3. 新建并启动容器

`docker [container] run`

```
[vagrant@localhost ~]$ docker run ubuntu /bin/echo 'Hello World'
Hello World
[vagrant@localhost ~]$
```

启动一个bash终端，允许用户进行交互：

```
[vagrant@localhost ~]$ docker run -it ubuntu /bin/bash
root@a637c2767dc2:/# pwd
/
root@a637c2767dc2:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@a637c2767dc2:/# ps
  PID TTY          TIME CMD
    1 pts/0    00:00:00 bash
   11 pts/0    00:00:00 ps
root@a637c2767dc2:/#
```

### 4. 守护态运行

```
[vagrant@localhost ~]$ docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
8153b7aef5ec6cf4015412d5a1187f992a361b0fd0b6acff69e900756931fbb2
[vagrant@localhost ~]$
```

### 5. 查看容器输出

`docker [container] logs`

```
[vagrant@localhost ~]$ docker logs 8153b7aef5ec6cf4015412d5a1187f992a361b0fd0b6acff69e900756931fbb2
hello world
hello world
hello world
hello world
hello world
```

## 二、停止容器

### 1. 暂停容器

`docker [container] pause CONTAINER [CONTAINER...]`

```
[vagrant@localhost ~]$ docker container pause 8153b7aef5ec
8153b7aef5ec
[vagrant@localhost ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS               NAMES
8153b7aef5ec        ubuntu              "/bin/sh -c 'while t…"   3 minutes ago       Up 3 minutes (Paused)                       elegant_proskuriakova
[vagrant@localhost ~]$
```

### 2. 终止容器

`docker [container] stop [-t|--time[=10]] [CONTAINER...]`

首先向容器发送SIGTERM信号，等待一段时间之后（默认为10秒），再发送SIGKILL信号来终止容器

```
[vagrant@localhost ~]$ docker stop 8153b7aef5ec
8153b7aef5ec
[vagrant@localhost ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### 3. 重新启动容器

`docker [container] start `

### 4. 重启容器

`docker [container] restart`

## 三、进入容器

推荐使用`exec`命令

### 1. attach命令

`docker [container] attach [--detach-keys[=[]]] [--no-stdin] [--sig-proxy[=true]] container`

### 2. exec 命令

`docker [container] exec CONTAINER COMMAND [ARG...]`

```
[vagrant@localhost ~]$ docker exec -it b179aadfa168 /bin/bash
root@b179aadfa168:/#
```

## 四、删除容器

`docker [container] rm CONTAINER`

## 五、导入和导出容器


`docker [container] export [-o|--output[=""]] CONTAINER`
`docker import file [REPOSITORY[:TAG]]`

```
[vagrant@localhost ~]$ docker export -o ubuntu.tar b179aadfa168
[vagrant@localhost ~]$ ls
example-voting-app  python3  ubuntu_latest.tar  ubuntu.tar
[vagrant@localhost ~]$ docker import ubuntu.tar rookie/ubuntu:latest
sha256:3596cc4b059c2892e0e1ab3df41aaf7f717ddf0f99fd1b7ede0cc3575741dec0
[vagrant@localhost ~]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
rookie/ubuntu       latest              3596cc4b059c        25 seconds ago      64.2MB
python              3                   3856488fc969        4 days ago          95.1MB
test                0.1                 a9c8eb4f717d        4 days ago          64.2MB
myubuntu            latest              3556258649b2        2 weeks ago         64.2MB
ubuntu              latest              3556258649b2        2 weeks ago         64.2MB
debian              stretch-slim        226ee7ba65a2        4 weeks ago         55.3MB
[vagrant@localhost ~]$
```

## 六、查看容器

### 查看容器详情

`docker container inspect [OPTIONS] CONTAINER [CONTAINER...]`

### 查看容器内的进程

`docker [container] top [OPTIONS] CONTAINER [CONTAINER...]`

### 查看统计信息

`docker [container] stats [OPTIONS] [CONTAINER...]`

### 复制文件

`docker [container] cp CONTAINER:SRC_PATH DEST_PATH`

### 查看变更

`docker [container] diff CONTAINER`

### 查看端口映射

`docker container port CONTAINER [PRIVATE_PORT[/PROTO]]`

### 更新配置

`docker [container] update CONTAINER`


