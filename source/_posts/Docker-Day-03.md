---
title: Docker-Day-03
categories: 学习
tags: Docker
---

## 初识Dockerfile

```shell
#创建一个dockerfile文件 名字随机
#文件中的内容 指令(大写) 参数
FROM centos
VOLUME ["volume01","volume02"]
CMD echo ----end---
CMD /bin/bash
```

```
docker build -f dockerfile -t zty/test:1.0 .
```

![image-20220421154923930](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220421154923930.png)

### 数据卷容器

```shell
docker run -it --name docker02 --volumes-from docker01 zty/test:1.0
```

## Dockerfile

### Dockerfile介绍

dockerfile是用来构建docker镜像的文件

构建步骤

1. 编写一个dockerfile 文件
2. docker build 构建成一个镜像
3. docker run 运行镜像
4. docker push 发布镜像（Dockerhub、阿里云镜像仓库）

## Dockerfile构建

**基础知识**

1. 每个保留关键字（指令）都必须大写
2. 执行从上到下，顺序执行
3. #表示注释
4. 每一个指令，都会创建提交一个新的镜像层，并提交

### Dockerfile指令

```shell
FROM                    #基础镜像
MAINTAINER              #镜像是谁写的  姓名+邮箱
RUN                     #镜像运行的时候需要运行的命令
ADD                     #添加文件
WORKDIR                 #镜像的工作目录
VOLUME                  #挂载的目录
EXPOST                  #暴露端口
CMD                     #指定容器启动要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT              #指定容器启动要运行的命令，追加命令
COPY                    #类似ADD,将文件拷贝到镜像中
ENV                     #构建的时候设置环境变量
```

## 实战

```shell
#编写配置文件
FROM centos
MAINTAINER zty<3562621613@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yun -y install vim

CMD /bin/bash
#构建
docker build -f dockerfile -t cc:1.0 .
```

### Dockerfile制作Tomcat镜像

- 准备tomcat压缩包、和jdk压缩包

![image-20220422135922194](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422135922194.png)

- 编写Dockerfile文件 官方命名**Dockerfile** build时会自动寻找这个文件 不需要加 -f

```shell
FROM centos:centos7
MAINTAINER zty<3562621613@qq.com>
ADD jdk-8u202-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.62.tar.gz /usr/local/

RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_202
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.62
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.62
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.62/bin/startup.sh

```

- 构建镜像

```shell
docker build -t zty .
```

- 启动，设置端口，并挂载

```shell
docker run -d -p 9090:8080 --name ztyzty -v /home/Docker-file-test/tomcat/test/:/usr/local/apache-tomcat-9.0.62/webapps/test -v /home/Docker-file-test/tomcat/logs/:/usr/local/apache-tomcat-9.0.62/logs zty
```

![image-20220422145630043](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422145630043.png)

- 发布项目

配置web-xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" 
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
</web-app>
```

再编写一个index.jsp文件

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 35626
  Date: 2022/3/10
  Time: 8:39
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="utf-8" %>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html" charset="UTF-8">
    <title>$Title$</title>
</head>
<body>
<form action="check.jsp" method="post">
    用户名：<input type="text" name="uname"><br>
    密码：<input type="password" name="pwd"><br>
    <input type="submit" value="提交"><br>
</form>
</body>
</html>

```

成功运行

![image-20220422154550938](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220422154550938.png)
