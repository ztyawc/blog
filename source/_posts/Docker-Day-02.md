---
title: Docker-Day-02
categories: 学习
tags: Docker
---

## **练习 ---nginx**

- 搜索镜像

```shell
docker search nginx
```

- 下载镜像

```shell
docker pull nginx
```

![image-20220420150327759](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420150327759.png)

- 查看镜像

```shell
docker images
```

![image-20220420150527289](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420150527289.png)

- 启动容器

```shell
docker run -d --name nginx01 -p 688:80 nginx

#解释
以后台方式启动nginx 并改名为nginx01 端口设置为688 (80为容器内端口，688为为外网访问地址)
```

- 查看运行中的容器

```shell
docker ps
```

![image-20220420151224241](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420151224241.png)

## Docker可视化

- **Portainer** docker的图形化界面管理工具

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

- 通过localhost:8088访问
- 设置账号密码

![image-20220420162036014](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420162036014.png)

- 选择本地

![image-20220420162356061](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420162356061.png)

## Docker镜像

### 镜像是什么？

- 镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，他包含

某个软件运行的所有内容，包括代码、运行时库、环境变量和配置文件。

- 如何得到镜像：

仓库下载

别人转发

自己制作（DockerFile）

### commit镜像

```shell
docker commit 提交容器成为一个新的副本
docker commit -m="提交描述信息" -a="作者" 容器id 目标镜像名:[tag]
```

![image-20220420171047597](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420171047597.png)

## 容器数据卷

### 使用数据卷

- 方式一用命令方式挂载

```shell
docker run -it -v 主机目录:容器目录

#测试 docker run -it -v /home/test:/home centos
```

![image-20220420173232237](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420173232237.png)

### 实战 Mysql

- 安装mysql5.7版本

```shell
docker pull mysql:5.7
```

- 配置

```shell
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

-d            后台运行
-p            映射端口
-v            映射目录
-e            设置mysql密码
--name        设置名字
```

### 具名和匿名挂载

- 演示

```shell
#匿名挂载
docker run -d -P --name nginx01 -v /etc/nginx nginx
#查看所有volume情况
root@VM-20-9-ubuntu:/home/mysql# docker volume ls
DRIVER    VOLUME NAME
local     55bafccffc2a6d8726e9fb1847d190dff6b213eb2d4787d734c709047d590dbb
local     702457b9f0bac968b8b259d0658f1ef1d27f13af02e650370fcb26eb8295fe55
#这是匿名挂载 -v 只有容器内路径 没有容器外路径

#具名挂载
docker run -d -P --name nginx02 -v zty:/etc/nginx nginx
#查看所有volume情况
root@VM-20-9-ubuntu:/home/mysql# docker volume ls
DRIVER    VOLUME NAME
local     55bafccffc2a6d8726e9fb1847d190dff6b213eb2d4787d734c709047d590dbb
local     702457b9f0bac968b8b259d0658f1ef1d27f13af02e650370fcb26eb8295fe55
local     zty
#查看卷
```

![image-20220420182756954](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220420182756954.png)

所有的docker容器内的卷，没有指定目录的情况下都是在**/var/lib/docker/volumes/xxx** 大多数情况使用**具名挂载**

- 拓展

```shell
docker run -d -P --name nginx02 -v ro zty:/etc/nginx nginx
docker run -d -P --name nginx02 -v rw zty:/etc/nginx nginx

#通过 -v ro rw 改变读写权限
#    ro      只读
#    rw      读写
```

