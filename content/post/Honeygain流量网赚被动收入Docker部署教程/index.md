---
title: 网络流量被动收入之Honeygain Docker部署教程
description: 被动收入之Honeygain Docker部署
date: 2023-01-16
slug: passive-income-honeygain
weight: 10
image: Passive-Income-Honeygain-Front.jpg
categories:
    - 赚钱项目
    - 网络流量
    - 被动收入
    - Honeygain
    - Docker
---

## Honeygain 是什么

[HoneyGain](https://www.honeygain.com/) 是一个通过共享自己闲置不用的网络带宽赚钱的平台。HoneyGain 支持多种不同的操作系统和设备，安装后既可自动分享闲置的网络带宽，分享的越多，赚得越多，是一个完全免费、简单易用、有网络就能用的 “躺赚” 被动收入平台。

HoneyGain 应该算是众多网络共享平台里**最流行的**，而且网上非常多的人已经真实的拿到了现金奖励，可以说是一个非常靠谱的平台。

新用户注册会送5美元奖励，[注册链接](https://r.honeygain.me/JJC27EFDE4)。

默认Honeygain是满20美元提现（通过`paypal`手续费较高），不过现在有一个`JumpTask`的模式，和电子货币平台合作（手续费较低，而且不限制提现额度），不过涉及电子货币需要对应的电子钱包。

## Honeygain 的收益如何

首先要先明确，使用`Honeygain`客户端跑的结果**并不是**直接的收益，而是`honeygain`的积分。按照官网说法，每1000积分对应的是1美元，而10MB流量对应3积分，也就是1GB大概307.2积分（3.25美元）。

![Honeygain官方收益换算规则](Honeygain-Incoming-Rule.png)

截止2023-01-16，笔者还没有提现，不过在`JumpTask`是能够按照操作提现到对应地址钱包的，所以提现应该问题不大。笔者从2022年大概9月、10月左右开始玩，一开始的收益（3个设备）有大概50-80积分是浮动，折算回来每天大概0.05-0.08美元/天，而且这个估计和国内网络有关，连接hoenygain的服务器质量比较差可能是，所以跑的流量稍微低一些。

## Honeygain 有什么限制

1. 需要家宽`IP`，否则会**运行失败**，提示`Not Resdential ip`
2. 同一个出口`IP`只能有一个设备挂着，否则会提示`OverUsed`
3. 国内`IP`访问可能会有时候连接失败，因为官网使用`Cloudflare CDN`有时候国内网络到海外会延迟较大，客户端访问`API`会报错，这也是影响收益的原因之一
4. 需要设备，手机的话会存在杀后台，建议还是家里的一直开启的设备，比如是软路由、微型主机等等功耗低、有一定内存和硬盘空间的设备。

## Honeygain 部署

Honeygain 目前支持MacOS、Android、Windows、IOS、Linux

![Honeygain支持平台](Honeygain-Support-Platform.png)

桌面端和移动端，可以直接通过[注册](https://r.honeygain.me/JJC27EFDE4)，登陆[Dashboard](https://dashboard.honeygain.com/#)，右上角可以自行下载，这里不展开讨论，都是按照步骤安装而已。

**备注**：个人建议优先推荐桌面端，能打开`Content Delivery`功能，理论上是`CDN`的功能，不过笔者目前还没有看到这个类别的收入。不过桌面端的客户端我这边跑了几个月，看起来分配的流量比容器部署更加多。

### Docker 容器化部署

Docker安装这里不展开描述，可以参考文章  [Docker安装部署](https://yysy.site/p/docker-installation/)

Honeygain一键安装命令

```shell
docker run -d --name honeygain --restart=always honeygain/honeygain -tou-accept -email <邮箱地址-账号名> -pass <密码> -device <设备名>
```

- `email` 参数后需要替换为你 `Honeygain` 对应的账号名，一般是邮箱地址
- `pass` 参数后需要替换为你 `Honeygain` 对应的密码
- `device` 参数后需要替换为你这个设备的名字，如果成功的话[Dashboard](https://dashboard.honeygain.com/#)页面中`My active devices`会看到在线

执行后可以通过命令行查看日志

```shell
docker logs -f honeygain
```

返回如下图

![Honeygain容器运行正常日志](Honeygain-Running-In-Docker.png)

## 结束语

如果有其他疑问或者部署上有什么难题，欢迎联系 [我](mailto:jjc27017@gmail.com) 或者加入 [讨论群](https://t.me/yysy_blog_chat) 一起讨论

其他网赚/网络流量挂机/被动收入项目：

- 
-  
-  
-  
