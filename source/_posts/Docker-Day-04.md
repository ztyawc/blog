---
title: Docker-Day-04
categories: 学习
tags: Docker
---

昨天试了一下dockerfile 里面安装不了vim啥的，最后查资料得出是centos8的锅

换成centos7就行了

然后是dockerfile里面安装东西都需要

```shell
yum -y
```

- 安装东西前的一些步骤

```shell
yum -y install epel-release
yum -y update
```

接下来就是今天的实战

dockerfile搭建个hexo

- hexo dockerfile文件

```shell
FROM centos:centos7
MAINTAINER zty<3562621613@qq.com>
RUN yum -y install epel-release
RUN yum -y update
RUN yum -y install vim
RUN yum -y install git-core
RUN yum -y install nodejs
RUN yum -y install npm

ENV MYPATH /usr/local
WORKDIR $MYPATH

EXPOSE 4000

RUN npm install -g hexo-cli
RUN hexo init fdgd

WORKDIR /usr/local/fdgd            #这里我原理用了cd命令后面发现没反应，得设置成WORKDIR才可以！！！！

CMD hexo g && hexo s
```

- 运行

```shell
docker run -d --name hexo -p 4000:4000 90609a5b1cc3
```

完美结束

## 发布镜像到Dockerhub

- 注册账号
- 在服务器上提交

```shell
root@VM-20-9-ubuntu:/home/Docker-file-test# docker login --help

Usage:  docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username
root@VM-20-9-ubuntu:/home/Docker-file-test# 

```

登陆成功

![image-20220422183401382](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422183401382.png)

- docker push

```shell
#先改名字
docker tag 90609a5b1cc3 ztyawc/hexo:1.0
#然后push
docker push ztyawc/hexo:1.0
```

![image-20220422184853270](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422184853270.png)
