---
title: Docker-Day-05
categories: 学习
tags: Docker
---

## Docker网络

### 理解Docker0

![image-20220422200914117](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422200914117.png)

```shell
# 查看容器内部地址
ip addr

```

![image-20220422212840217](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422212840217.png)

docker和容器是可以互相ping通的



而两个容器又是ping不通的

![image-20220423125308647](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220423125308647.png)

这时需要--link

```shell
docker run -d --name tomcat03 -P --link tomcat02 tomcat:8.0

docker exec -it tomcat03 ping tomcat02

#这时候发现就可以ping通了
```

![image-20220423125836513](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220423125836513.png)

```shell
docker exec -it tomcat02 ping tomcat03      #反向却不可以

root@VM-20-9-ubuntu:/home/ubuntu# docker exec -it tomcat02 ping tomcat03
ping: unknown host tomcat03
```

原理

```shell
root@VM-20-9-ubuntu:/home/ubuntu# docker exec -it tomcat03 cat /etc/hosts    #查看tomcat03hosts配置
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.3	tomcat02 e5ff12c5d040              #其实就是把tomcat02 配置进去
172.17.0.4	a476e2d64c40


#这样配置就可以ping通(需要重启容器)
```

###  自定义网络

```shell
#查看网卡配置帮助命令
root@VM-20-9-ubuntu:/home/ubuntu# clear
root@VM-20-9-ubuntu:/home/ubuntu# docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more network
  
  
#查看所有的网络
root@VM-20-9-ubuntu:/home/ubuntu# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
c2a014459fae   bridge    bridge    local
d4d63bee5ed6   host      host      local
b0aafb41ed08   none      null      local
```

**网络模式：**

bridge：     桥接（默认）

 none：      不配置网络

host：         和宿主机共享网络

container： 容器网络连通（用的少，局限性大）

**测试**

```shell
docker run -d --name tomcat01 -P tomcat:8.0

#等于

docker run -d --name tomcat01 -P --net bridge tomcat:8.0

#docker0特点：默认，且域名不能访问   --link可以打通链接

#我们可以自定义一个网络

--driver bridge
--subnet 192.168.0.0/16
--gateway 192.168.0.1

root@VM-20-9-ubuntu:/home/ubuntu# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
e6d27b7e86d45564f17d6318fa3ef781f0910421f99300281035f680f20ae0a6

```

docker network **inspect** mynet

![image-20220423133734527](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220423133734527.png)

用主机搭建的网络来启动容器

```shell
root@VM-20-9-ubuntu:/home/ubuntu# docker run -d --name tomcat01 -P --net mynet tomcat:8.0
fc7f2e08fac0442fba57befb24ac4219ba94f5879260d487c18f0f48973eb37a

root@VM-20-9-ubuntu:/home/ubuntu# docker run -d --name tomcat02 -P --net mynet tomcat:8.0
36e031d94238bb27586f832192b0416fb7cef027aec55db8b1efe07fb24e226a
```

![image-20220423134311476](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220423134311476.png)

这时候两个容器不需要--link也可以**互相**ping通

### 网络联通

```shell
docker network connect mynet tomcat01
```

