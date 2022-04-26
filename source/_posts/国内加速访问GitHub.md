---
title: 国内加速访问GitHub
cover: https://i.imgtg.com/2022/02/15/TuajG.md.jpg
---

### 原理

绕过国内DNS解析，直接访问GitHub的CDN节点，从而达到加速的目的。该方法也可加速其他因为CDN被屏蔽导致访问慢的网站。

大白话：其实就是不用默认DNS解析到的IP，而是直接指定一个IP去访问；比如：你去访问github.com，默认的DNS给你解析到52.74.223.119，而我用hosts文件给系统指定了一个IP，以后再访问github.com，就直接去访问我配置的那个IP。

所以如果你配置的那个IP就成了访问效果好坏的关键

## 实现

分别查询以下三个链接的DNS解析地址：

- github.com
- assets-cdn.github.com
- github.global.ssl.fastly.net

查询DNS的解析网站有：

[站长工具](https://tool.chinaz.com/dns)

然后修改**hosts**文件

Windows系统：进入目录：`C:\Windows\System32\drivers\etc\`，复制 `hosts`文件 到桌面，用编辑器打开，在末尾加上刚才查询到的IP和对应的域名，格式为：`IP (空格) 域名`。我的hosts文件

![image-20220418202434141](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220418202434141.png)

保存之后再将这个文件复制到目录：`C:\Windows\System32\drivers\etc\`中，会提示拒绝访问，用管理员点确认即可 

然后再手动刷新系统DNS缓存，命令：Win+R cmd ，输入`ipconfig /flushdns` 即可

![image-20220418202623589](https://cdn.jsdelivr.net/gh/ztyawc/imgs/img/image-20220418202623589.png)