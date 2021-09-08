---
title: Docker 搭建 Jenkins
abbrlink: 770a69f8
date: 2020-11-29 21:39:49
tags:
---



![](https://camo.githubusercontent.com/a5004ae5bffb9a59384514fd88d3f18c47e1e0373bfda94a18b422e4a164d399/68747470733a2f2f6a656e6b696e732e696f2f73697465732f64656661756c742f66696c65732f6a656e6b696e735f6c6f676f2e706e67)





## 1.安装 Docker



### Docker 简介

[Docker CE](https://www.docker.com/community-edition) 是一个开源的应用级别的虚拟化工具，可以将任何应用包装在"[LXC容器](Docker CE](https://www.docker.com/community-edition) 是一个开源的应用级别的虚拟化工具，可以将任何应用包装在"[LXC容器)”中运行。

> Linux 容器：Linux Containers

<!--more-->

### 安装 curl 工具

使用 [APT] 包管理工具安装 [cURL]：

```
apt update && apt install -y curl
```

> APT: Debian Linux 的默认包管理工具

> cURL: 最常用的命令行文件传输工具

### 安装 Docker

官方已经给出了适合 Linux 平台的自动安装脚本。因此想要安装 Docker，只需要运行下面的命令：

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

[?] 在上面的命令中，我们添加了[参数] `--mirror` 以使用[国内的安装包镜像]。



> --mirror Aliyun

> 官方脚本支持的镜像参数有: Aliyun、AzureChinaCloud

> curl -fsSL [https://get.docker.com](https://get.docker.com/) | bash -s docker --mirror Aliyun

## 2.添加 Docker Hub 镜像加速

您可以通过修改 daemon.json 配置文件来使用 Docker Hub 加速服务。

### 创建 daemon.json 文件

请在 `/etc/docker` 目录下创建 *daemon.json*。

```
touch /etc/docker/daemon.json
```

### 编辑 daemon.json 文件

*编辑 /etc/docker/daemon.json*，添加镜像服务地址。腾讯云镜像的配置如下：

##### 示例代码：/etc/docker/daemon.json

```json
{
    "registry-mirrors": ["https://mirror.ccs.tencentyun.com"]
}
```

### 重新启动 Docker

```
systemctl daemon-reload
systemctl restart docker
```

## 3.安装 Jenkins

### Jenkins 简介

[Jenkins](https://jenkins.io/) 是一个开源软件项目，是基于 Java 开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

### 获取 Jenkins 镜像

Jenkins 官方镜像主要提供两个版本，分别是 [LTS] 版和[每周更新]版。本教程使用的是 LTS 版。

```
docker pull jenkins/jenkins:lts
```

> Jenkins 发布的长期支持版：Long-term support

> Jenkins 每周发布一期软件更新的版本：weekly

### 启动 Jenkins 容器

使用 Docker 在宿主主机启动 Jenkins 容器：

```
docker run --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --restart always -d jenkins/jenkins:lts
```

#### docker run 附加参数说明：

| 附加参数  | 本教程所使用的值 | 用途                                                         |
| --------- | ---------------- | ------------------------------------------------------------ |
| --name    | jenkins          | [命名] |
| -p        | 8080:8080        | [端口映射] |
| -v        | jenkins_home     | [数据卷] |
| --restart | always           | [重启策略] |

有关更多配置方法请查看[官方 Docker 文档](https://github.com/jenkinsci/docker/blob/master/README.md)。

> 容器的名称: --name jenkins

> 容器到宿主主机的端口映射: -p 8080:8080 -p 50000:50000

> 数据卷挂载: -v jenkins_home:/var/jenkins_home

> 退出时总是重启容器: --restart always

### 访问 Jenkins 实例

在浏览器中打开 [http://<您的  IP 地址>:8080]即可浏览之前启动的 Jenkins 实例。

### 获取初始登录密码

回到命令行，执行以下命令：

```
docker exec jenkins \
cat /var/jenkins_home/secrets/initialAdminPassword
```

从输出结果中获得的一串 Jenkins 初始密码，复制密码，回到 [http://<您的 IP 地址>:8080] 填入密码。

### 定制 Jenkins

我们选择默认的 **Install suggested plugins** 来安装插件。

![](/assets/blogImg/customize-jenkins.png)

### 创建帐户

请根据引导页面的信息创建第一个管理员用户，之后即可开启 Jenkins 的世界。

![](/assets/blogImg/create-first-admin-user.png)

### 完成

恭喜，您已完成 Docker 搭建 Jenkins 