---
title: Hexo 搭建总结
cover: https://cdn.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg.png
---

## 安装

### 安装git

- Windows：[点我](https://git-scm.com/download/win)

- Linux (Ubuntu, Debian)：

  ```shell
  sudo apt-get install git-core
  ```

- Linux (Fedora, Red Hat, CentOS)：

  ```shell
  sudo yum install git-core
  ```

### 安装 Node.js

- Windows：通过 [nvs](https://github.com/jasongin/nvs/)（推荐）或者 [nvm](https://github.com/nvm-sh/nvm) 安装

- Linux（DEB/RPM-based）：从 [NodeSource](https://github.com/nodesource/distributions) 安装

### 安装 Hexo

```shell
npm install -g hexo-cli
```

安装后添加到环境变量

```
npm root -g 
获取node_modules地址
/Users/guo/.npm-global/lib/node_modules
```

添加进PATH

```
$export PATH=$PATH:/Users/guo/.npm-global/lib/node_modules/hexo-cli/bin
```

## 建站

### 初始化

- 安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件

```shell
$ hexo init <folder>
$ cd <folder>
$ npm install
```

- 部署网站

```
hexo g
```

- 运行网站

```
hexo s
```

- 指定端口

```
hexo s -p 80
```

### Hexo后台运行

- 安装PM2

```
npm  install -g pm2
```

- 在博客根目录下面创建一个js文件

```js
const { exec } = require('child_process')
exec('hexo server',(error, stdout, stderr) => {
        if(error){
                console.log('exec error: ${error}')
                return
        }
        console.log('stdout: ${stdout}');
        console.log('stderr: ${stderr}');
})
```

- 运行js文件

```shell
pm2 start x.js
```

- 查看pm2任务列表

```shell
pm2 list
```

- 停止所有pm2任务

```shell
pm2 stop all
```

- pm2 开机自启

```shell
 pm2 startup
```
