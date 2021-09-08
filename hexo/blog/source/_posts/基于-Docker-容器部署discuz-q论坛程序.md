---
title: 基于 Docker 容器部署discuz q论坛程序
toc: true
tags:
  - discuz Q
  - Docker
categories:
  - CentOS
abbrlink: 1e39b68a
date: 2021-03-06 10:12:01
---

![](/assets/blogImg/202103061218.jpg)


#### 操作场景

本文档将指导您如何在已安装 docker-ce 运行环境的 Linux 服务器上安装部署 Discuz! Q。

> TIP
>
> 如果您的服务器还未安装 docker-ce 运行环境，腾讯云提供了 [docker 镜像 (opens new window)](https://mirrors.tencent.com/docker-ce/)，您可直接下载安装。

#### 前提条件

已成功登录 Linux 服务器。

#### 操作步骤

#### 步骤一：安装 Discuz! Q


<!--more-->
1. 在终端中，请输入以下命令。docker 将会自动下载并运行最新版本的 Discuz! Q。如下所示：

```text
docker run -d -p 80:80 -p 443:443 ccr.ccs.tencentyun.com/discuzq/dzq:latest
```

> WARNING
>
> - 容器基于 Ubuntu 18.04，其中已安装 Nginx 1.14, PHP 7.2, MySQL 5.7 和所有的相关依赖，并且已经完成 Web 服务器配置和计划任务配置。
> - 安装时警告 `WARNING: IPv4 forwarding is disabled. Networking will not work.`。可使用命令 `vim /etc/sysctl.conf` 编辑配置文件。修改 `net.ipv4.ip_forward`字段值为 `1` 开启转发并使用命令 `systemctl restart network` 重启网络服务。
> - 以上命令用于快速启动并测试 Discuz! Q，数据库和站点数据都将保存在容器内部，容器被删除将会造成数据丢失。
> - 如果您想基于容器长期运行Discuz! Q，建议将数据库和站点数据保存于容器外部，请参考[容器的更多配置说明进行配置 (opens new window)](https://discuz.com/docs/常见问题.html#容器的更多配置说明)。

#### 步骤二：初始化安装 Discuz! Q

1. 打开本地浏览器，访问 `http://<服务器外网 IP 地址>/install` 并配置网站相关信息。如下图所示：

![](/assets/blogImg/202103061017.png)


- **站点名称**：请输入您的站点名称信息，可自定义。
- **MySQL Host**：请输入您的 MySQL 服务器地址，如您使用 docker 创建的本地数据库，请输入`127.0.0.1`即可。
- **MySQL 数据库**：请输入您的数据库名称。如您使用 docker 创建的本地数据库，请输入`root`即可。
- **MySQL 用户名**：请输入您的数据库用户名。如您使用 docker 创建的本地数据库，请输入`root`即可。
- **MySQL 密码**：如您使用 docker 创建的本地数据库，请输入`root`即可。
- **表前缀**：可选，可自定义数据库表前缀名称。默认不填。
- **管理员 用户名**：请输入您 Discuz! Q 站点的管理员用户名。
- **管理员 密码**：请输入您 Discuz! Q 站点的管理员密码。
- **管理员 确认密码**：请再次输入您 Discuz! Q 站点的管理员密码。

> TIP
>
> - 如果您想基于容器长期运行 Discuz! Q，建议将数据库和站点数据保存于容器外部，请参考 [容器的更多配置说明进行配置 (opens new window)](https://discuz.com/docs/常见问题.html#容器的更多配置说明)。



#### 1.1 容器的更多配置说明

**如何将数据保存到容器外部** 本容器支持以下三个外部映射目录：

- 数据库文件，映射到 `/var/lib/mysqldb/`。
- Discuz! Q的配置与存储目录，映射到 `/var/lib/discuz/`。
- SSL证书文件，映射到 `/etc/nginx/certs/`，其中**要求**存在两个文件 `discuz.crt` 和 `discuz.key`。如果不使用SSL协议，请不要配置此目录，并且不映射 443 端口。

因此，如果如果您想长期使用容器来运行 Discuz! Q，建议在启动容器的时候加入这三个参数进行映射。

例如数据库文件，在本地(宿主机)上，想保存到 `/data/mysql-data`，Discuz! Q的运行数据，保存到 `/data/discuz`，SSL证书文件放在 `/data/certs/discuz.crt` 和 `/data/certs/discuz.key`，同时不想对外开放 80 端口，那启动容器的命令就是：

```text
docker run -d --restart=always \
  -p 443:443 \
  -v /data/discuz:/var/lib/discuz \
  -v /data/mysql-data:/var/lib/mysqldb \
  -v /data/certs:/etc/nginx/certs \
  ccr.ccs.tencentyun.com/discuzq/dzq:latest
```

启动之后，访问 `https://<域名>/install` 就可以开始安装，并正常使用。

> WARNING
>
> 请一定要访问外部用户将要访问的协议( `http://` 或 `https://` ) 加 **域名** 加 `/install` 进行安装，否则会导致自动获取的站点 URL 配置不正确，站点工作不正常。

####  1.2 基于容器的升级

只要将数据保存到了容器外部，容器就可以升级。在升级前，要将原容器先停止并删除(执行此命令时，一定要确保自己已经将数据保存到了容器外部)。

```text
docker stop <容器 ID>
docker rm <容器 ID>
```

其中的`<容器 ID>`，可以通过 `docker ps` 命令看到。 然后用以下命令下载最新版本镜像：

```text
docker pull ccr.ccs.tencentyun.com/discuzq/dzq:latest
```

再使用上次启动相同的命令重新启动即可。

如果需要执行升级文档中要求的其它升级命令，请先登录容器

```text
docker exec -it <容器 ID> /bin/bash
```

然后就可以执行升级文档中要求的相关的命令，例如：

```text
 cd /var/www/discuz
 php disco migrate --force
```

**基于容器的一些其它配置**

- 如果您想对 mysql 进行管理，可选择以下两种方法之一：
  - 登录进容器，用 mysql 命令进行管理 `docker exec -it <容器id> /bin/bash`。
  - 将 3306 端口暴露到外面，通过外部工具连上去进行管理。在启动时，加一个 `-p 3306:3306`。
- 如果您想通过外部的负载均衡进行 SSL 卸载，可开放容器的 80 端口，不开放 443 端口即可。
- Nginx 的配置文件，位于容器的  `/etc/nginx/nginx.conf` 下，如果需要修改，可通过 `-v`映射自己的配置文件，覆盖这个文件。
  - 例如您本地的配置文件为 `/data/nginx.conf` ，可以在上面的启动命令中，加入映射： `-v /data/nginx.conf:/etc/nginx/nginx.conf`，即可覆盖系统原来内置的 Nginx 配置文件。
- `php-fpm` 的配置文件，位于容器的 `/etc/php/7.2/fpm/pool.d/www.conf`，也可同样映射修改。
- 控制 PHP 上传大小的文件，位于容器的 `/etc/php/7.2/fpm/conf.d/30-upload-size.ini`, 当前设置为20M，可同样映射修改。



> TIP
>
> **基于自己的服务器部署docker 启动的命令：**
>
> docker run -d --restart=always \
>   -p 443:443 \
>   -p 3307:3306 \
>   -v /data/nginx.conf:/etc/nginx/nginx.conf \
>   -v /data/discuz:/var/lib/discuz \
>   -v /data/mysql-data:/var/lib/mysqldb \
>   -v /data/certs:/etc/nginx/certs \
>   ccr.ccs.tencentyun.com/discuzq/dzq:latest


<a href="/assets/book/nginx.conf" target="_blank">点击下载nginx.conf配置文件</a>


2. 单击【安装】。即可完成 DIscuz！Q 的安装部署。

#### 步骤三：系统管理与配置

安装完成后，您可以访问 `http://<服务器外网 IP 地址>/admin` 进入后台，输入在安装的时候设置的管理员账号和密码，进行管理与配置。