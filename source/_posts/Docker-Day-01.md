---
title: Docker-Day-01
categories: 学习
tags: Docker
---

## 安装Docker

**ubuntu版本!**

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

- 启动Docker


```shell
systemctl start docker
```

使用 docker version来验证是否安装成功

![image-20220419161737407](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220419161737407.png)

- hello-world


```shell
docker run hello-world
```

![image-20220419162130286](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220419162130286.png)

- 查看docker镜像


```shell
docker images
```

![image-20220419162251186](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220419162251186.png)

## 卸载Docker

```shell
apt-get purge docker-ce docker-ce-cli containerd.io
```

```shell
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

## Docker的常用命令

### 帮助命令

```shell
docker version      #显示docker的版本信息
docker info         #查看docker的系统信息
docker 命令 --help  #帮助命令
```

### 镜像命令

- **docker images** 查看本地所有镜像

```shell
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   6 months ago   13.3kB

#解释
REPOSITORY   镜像的仓库源
TAG          镜像的标签
IMAGE ID     镜像的ID
CREATED      镜像的创建时间
SIZE         镜像的大小

#可选项
  -a, --all             #列出所有镜像
  -q, --quiet           #只显示镜像的ID 
```

- **docker search** 搜索镜像

```shell
NAME                        DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
whyour/qinglong                                                             534                  
drewnb/qinglong             时间管理器，备份青龙                                      58                   

#可选项
root@VM-20-9-ubuntu:/home/ubuntu# docker search mysql --filter=STARS=5000 #只搜索STARS大于5000的
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   12433     [OK]    
```

- **docker pull** 下载镜像

```shell
docker pull mysql                       #下载镜像
Using default tag: latest               #如果不写tag 则默认为latest
latest: Pulling from library/mysql
f003217c5aae: Pull complete             #分层下载 docker image核心 联合文件系统
65d94f01a09f: Pull complete 
43d78aaa6078: Pull complete 
a0f91ffbdf69: Pull complete 
59ee9e07e12f: Pull complete 
04d82978082c: Pull complete 
70f46ebb971a: Pull complete 
db6ea71d471d: Pull complete 
c2920c795b25: Pull complete 
26c3bdf75ff5: Pull complete 
9ec1f1f78b0e: Pull complete 
4607fa685ac6: Pull complete 
Digest: sha256:1c75ba7716c6f73fc106dacedfdcf13f934ea8c161c8b3b3e4618bcd5fbcf195  #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest             #真实地址
#等价于
docker pull mysql
docker pull docker.io/library/mysql:latest 

#指定版本下载
docker pull mysql:5.7
5.7: Pulling from library/mysql
f003217c5aae: Already exists 
65d94f01a09f: Already exists 
43d78aaa6078: Already exists 
a0f91ffbdf69: Already exists 
59ee9e07e12f: Already exists 
04d82978082c: Already exists 
70f46ebb971a: Already exists 
ba61822c65c2: Pull complete 
dec59acdf78a: Pull complete 
0a05235a6981: Pull complete 
c87d621d6916: Pull complete 
Digest: sha256:1a73b6a8f507639a8f91ed01ace28965f4f74bb62a9d9b9e7378d5f07fab79dc
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

![image-20220419172701283](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220419172701283.png)



- **docker imi** 删除镜像

```shell
docker rmi -f 镜像id                      #删除指定的镜像
docker rmi -f 镜像id 镜像id 镜像id         #删除多个镜像
docker rmi -f $(docker images -aq)       #删除所有的镜像
```



### 容器命令

**有了镜像才可以使用容器 linux 下载一个centos镜像来学习**

```shell
docker pull centos
```

- **新建容器并启动**

```shell
docker run [可选参数] image

#参数说明
-it                 使用交互方式运行 进去容器查看内容
--name="name"       容器名字
-d                  后台方式运行
-p                  指定的容器端口8080:8080
      -p ip:主机端口:容器端口
      -p 主机端口:容器端口(常用)
      -p 容器端口
 #启动并进入容器
root@VM-20-9-ubuntu:/home/ubuntu# docker run -it centos
[root@cd847ed554e5 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
#退出容器
exit

```

- **列出所有运行的容器**

```shell
docker ps
docker ps -a     #列出历史运行的容器
docker ps -aq    #列出历史运行的容器id
```

-  **退出容器**

```shell
exit        #直接容器停止并退出
Ctrl+p+q    #退出但不停止容器
```

- **删除容器**

```shell
docker rm 容器id                    #删除指定容器
docker rm -f $(docker ps -aq)      #删除所有容器
docker ps -a -q|xargs docker rm    #删除所有容器
```

- **启动和停止容器的操作**

```shell
docker start 容器id          #启动容器
docker restart 容器id        #重启容器
docker stop 容器id           #停止当前运行的容器
docker kill 容器id           #强制停止运行容器
```

### 其他常用命令

- **后台启动容器**

```shell
docker run -d 镜像名    docker run -d centos
#docker ps 的时候发现是停止的 所以容器里面必须要有一个前台应用
```

- **查看日志**

```shell
docker logs -tf --tail 10 a56829b9197d #查看容器的前10条日志
docker logs -tf a56829b9197d #查看容器的所有日志
```

- **查看容器的进程信息**

```shell
docker top 容器id
```

- **查看容器的元数据**

```shell
docker inspect 容器id
```

- **进去当前正在运行的容器**

```shell
docker exec -it 容器id /bin/bash    #新的窗口
docker attach 容器id                #正在运行的窗口
```

- **容器内文件拷贝到主机上**

```shell
docker cp 容器id:容器内路径 目的地自己路径    #docker cp a4b896d31adf:/home/zty.txt /home
```

