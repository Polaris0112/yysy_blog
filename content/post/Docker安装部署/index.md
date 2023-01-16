---
title: Docker安装部署
description: Docker安装部署过程
date: 2023-01-16
slug: docker-installation
weight: 5
image: Docker-Installation-Front.jpg
categories:
    - 部署
    - Docker
    - 技术文档
---

## Docker 是什么

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

Docker 从 17.03 版本之后分为 CE（Community Edition: 社区版） 和 EE（Enterprise Edition: 企业版），我们用社区版就可以了。

[Docker 官网](https://www.docker.com/)

[Github Docker 社区版源码](https://github.com/docker/docker-ce)

## Docker 各平台部署

### Windows

[Windows 64位官网下载地址](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)

`EXE`文件，按步骤安装即可，是桌面版，有UI，也可以使用`powershell`的`docker`命令管理

### MacOS

[Mac DMG包官网下载地址](https://desktop.docker.com/mac/main/amd64/Docker.dmg)

`DMG`文件，按步骤安装即可，是桌面版，有UI，也可以使用终端的`docker`命令管理

### Linux

个人比较推荐脚本一键安装，自动判断系统然后进行安装，也可以添加对应镜像加速器

安装命令如下：

```
## 阿里云作为镜像加速
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

## 微软云作为镜像加速
curl -fsSL https://get.docker.com | bash -s docker --mirror AzureChinaCloud
```

也可以使用国内 daocloud 一键安装命令：

```
curl -sSL https://get.daocloud.io/docker | sh
```

一句话命令，如果顺利跑完的话，最后会显示docker相关信息，即已经安装完成。

### （可选）国内镜像加速

国内由于连接国外网络原因，所以一般需要加速代理

- 科大镜像：**https://docker.mirrors.ustc.edu.cn/**
- 网易：**https://hub-mirror.c.163.com/**
- 阿里云：**https://<你的ID>.mirror.aliyuncs.com**
- 七牛云加速器：**https://reg-mirror.qiniu.com**

修改文件`/etc/docker/daemon.json`（没有的话就新建），文件内容为

```json
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com/"
  ]
}
```

**注意**：一定要保证该文件符合 json 规范，否则 Docker 将不能启动。

之后重新启动服务

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

检查加速器是否生效

```shell
docker info
```

如果从结果中看到了如下内容，说明配置成功

```shell
Registry Mirrors:
 https://hub-mirror.c.163.com/
```

## 结束语

Docker容器作为一个用于开发，交付和运行应用程序的开放平台，能够将应用程序与基础架构分开，从而可以快速交付软件。而且很多平台搭建或者软件工具，因为环境的差异性，导致我们运行起来会有一定难度，所以容器的部署是我们之后教程的一个重要基础。

如有什么疑问可以进入[讨论群](https://t.me/yysy_blog_chat)一起讨论解决。
