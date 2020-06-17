---
title: 搭建 FTP 文件服务
toc: true
tags:
  - FTP
  - CentOS
categories:
  - CentOS
abbrlink: 27950b90
date: 2020-06-15 22:09:28
---



![](/assets/blogImg/202006151712.jpg)



**摘要**

FTP 是一个很实用的文件传输协议，方便在客户端和服务器之间进行文件的传输。本文章带您使用 vsftpd 来搭建一个 FTP 服务，并且创建专有的 FTP 登录账户，保障服务器安全。


**环境**

CentOS 7.2 64 位

<!--more-->



## 1、安装并启动 FTP 服务

 **安装 VSFTPD**

使用 `yum` 安装 [[vsftpd](https://cloud.tencent.com/developer/labs/lab/10002#stage-1-step-1-vsftpd)]：

```
yum install vsftpd -y
```



> `vsftpd` 是在 Linux 上被广泛使用的 FTP 服务器，根据其[官网介绍](https://security.appspot.com/vsftpd.html)，它可能是 UNIX-like 系统下最安全和快速的 FTP 服务器软件。

 **启动 VSFTPD**

安装完成后，启动 FTP 服务：

```
service vsftpd start
```

启动后，可以看到系统已经[[监听了 21 端口](https://cloud.tencent.com/developer/labs/lab/10002#stage-1-step-2-21)]：

```
netstat -nltp | grep 21
```

此时，访问 [ftp://<您的  IP 地址>](ftp://xn--< cvm ip -sd04ayi737nv69f/) 可浏览机器上的 */var/ftp* 目录了。



> FTP 协议默认使用 21 端口作为服务端口

## 2、 配置 FTP 权限

目前 FTP 服务登陆允许匿名登陆，也无法区分用户访问，我们需要配置 FTP 访问权限

**了解 VSFTP 配置**

vsftpd 的配置目录为 */etc/vsftpd*，包含下列的配置文件：

- *vsftpd.conf* 为主要配置文件
- *ftpusers* 配置禁止访问 FTP 服务器的用户列表
- *user_list* 配置用户访问控制

阅读上述配置以了解更多信息。如果您准备好了，点击下一步开始修改配置来设置权限。

 **阻止匿名访问和切换根目录**

匿名访问和切换根目录都会给服务器带来[[安全风险](https://cloud.tencent.com/developer/labs/lab/10002#stage-2-step-2-safety)]，我们把这两个功能关闭。

*编辑 /etc/vsftpd/vsftpd.conf*，[[找到下面两处配置](https://cloud.tencent.com/developer/labs/lab/10002#stage-2-step-2-find)]并修改：

```
# 禁用匿名用户
anonymous_enable=NO

# 禁止切换根目录
chroot_local_user=YES
```

编辑完成后，按 `Ctrl + S` 保存配置，重新启动 FTP 服务，如：

```
service vsftpd restart
```



> 匿名访问让所有人都可以上传文件到服务器上而无需鉴权，而允许切换根目录则可能产生越权访问问题。



> 在代码编辑器中，用 `Ctrl + F` 进行搜索，Mac 用户用 `Cmd + F` 进行搜索

 **创建 FTP 用户**

创建一个用户 `ftpuser`：

```
useradd ftpuser
```

为用户 `ftpuser` 设置密码：

```
echo "Password" | passwd ftpuser --stdin
```



> 为了方便后面的实验步骤，不建议使用其它的用户名



> 下面命令中的密码为实验室为您生成，为了方便后面的实验步骤，不建议使用其他密码

 **限制该用户仅能通过 FTP 访问**

限制用户 `ftpuser` 只能通过 FTP 访问服务器，而不能直接登录服务器：

```
usermod -s /sbin/nologin ftpuser
```

 **为用户分配主目录**

为用户 `ftpuser` 创建[[主目录](https://cloud.tencent.com/developer/labs/lab/10002#stage-2-step-5-ftp-home)]并约定：

`/data/ftp` 为主目录, 该目录不可上传文件

`/data/ftp/pub` 文件只能上传到该目录下

```
mkdir -p /data/ftp/pub
```

创建登录欢迎文件：

```
echo "Welcome to use FTP service." > /data/ftp/welcome.txt
```

设置访问权限：

```
chmod a-w /data/ftp && chmod 777 -R /data/ftp/pub
```

设置为用户的主目录：

```
usermod -d /data/ftp ftpuser
```



> 用户的主目录是用户通过 FTP 登录后看到的根目录



> 方便用户登录后可以看到欢迎信息，并且确定用户确实登录到了主目录上。

## 3、准备域名和证书

注：如果您不需要通过域名访问 FTP 服务器则可以直接点击“已完成，下一步”跳过域名和证书的准备环节

 **1）域名注册**

可以到域名服务商去注册域名，例如：阿里云或者腾讯云都可以。

  **2) 域名解析**

域名购买完成后, 需要将域名解析到云主机的 IP上。

域名设置解析后需要过一段时间才会生效，通过 `ping` 命令检查域名是否生效，如：

```
ping yourdomain.com
```

如果 ping 命令返回的信息中含有你设置的解析的 IP 地址，说明解析成功。

（使用 `ctrl + c` 停止）

> 注意替换下面命令中的 `yourdomain.com` 为您自己的注册的域名

## 4、访问 FTP 服务

FTP 服务已安装并配置完成，下面我们来使用该 FTP 服务

**访问 FTP 服务**

根据您个人的工作环境，选择一种方式来访问已经搭建的 FTP 服务

**通过 Windows 资源管理器访问**

Windows 用户可以复制下面的[[链接](https://cloud.tencent.com/developer/labs/lab/10002#stage-4-step-1-address)]到资源管理器的地址栏访问：

```
ftp://ftpuser:Password@<您的 IP 地址>
```

 **通过 FTP 客户端工具访问**

FTP 客户端工具众多，下面推荐两个常用的：

- *WinSCP* - Windows 下的 FTP 和 SFTP 连接客户端
- *FileZilla* - 跨平台的 FTP 客户端，支持 Windows 和 Mac

下载和安装 FTP 客户端后，使用下面的凭据进行连接即可：

[[主机](https://cloud.tencent.com/developer/labs/lab/10002#stage-4-step-1-host)]：

```
<您的 IP 地址>
```

用户：

```
ftpuser
```

密码：

```
Password
```

如果能够正常连接，那么大功告成，您可以开始使用属于您自己的 FTP 服务器了！

接下来，请上传任意一张图片到您的 FTP 服务器上的pub目录下，然后，就可以在 */data/ftp/pub* 中看到了。

注意: `请不要直接上传文件到根目录下`，您应该选择上传到 `pub` 目录下



> 如果您申请了域名，可以将链接中的 Ip 地址替换为对应的域名访问 FTP 服务



> 如果您申请了域名，可以将Ip 地址替换为对应的域名作为访问凭据

### 大功告成

恭喜！您已经成功完成了搭建 FTP 服务器的任务。