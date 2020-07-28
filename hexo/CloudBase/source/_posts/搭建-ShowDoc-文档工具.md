---
title: 搭建 ShowDoc 文档工具
toc: true
tags:
  - ShowDoc
  - CentOS
categories:
  - CentOS
abbrlink: d72be85e
date: 2020-06-15 22:32:58
---



![](/assets/blogImg/202006151712.jpg)



**摘要**


程序员都很希望别人能写文档，而自己却不愿意写文档。文档的编写和管理影响了团队沟通协作的效率，ShowDoc 是一个非常适合 IT 团队的在线文档分享工具，为提升团队之间的沟通协作效率而生。本实验带您在 centos 系统上搭建基于 Nginx + PHP 的 ShowDoc 文档工具。



**环境**

CentOS 7.2 64 位

个人搭建的showdoc免费开放使用，地址是[http://zhaozf.site/showdoc/web/#/](http://zhaozf.site/showdoc/web/#/)
<!--more-->





## 1、准备 Nginx + PHP 环境

 **安装 Nginx**

使用 `yum` 安装 Nginx：

```
yum install nginx
```

修改 */etc/nginx/nginx.conf* 文件为如下内容：

```
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        server_name  127.0.0.1;
        root         /var/www/html;
        index index.php index.html
        error_page  404              /404.html;
        location = /40x.html {
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
        }
        location ~ .php$ {
            root           /var/www/html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
        location ~ /.ht {
            deny  all;
        }
    }
}
```

启动 Nginx 并设置为开机启动：

```
systemctl start nginx
systemctl enable nginx.service
```

 **安装 PHP**

使用 `yum` 安装 php-fpm：

```
yum install php php-gd php-fpm php-mcrypt php-mbstring php-mysql php-pdo
```

启动 php-fpm 并设置为开机启动：

```
systemctl start php-fpm
systemctl enable php-fpm.service
```

## 2、创建项目

**下载安装 Composer**

Composer 是 PHP 的一个依赖管理工具，推荐使用 Composer 创建 ShowDoc 项目。

执行如下命令[[安装 Composer](https://cloud.tencent.com/developer/labs/lab/10108#stage-2-step-1-install-composer)]：

```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```



> 安装过程可能需要耗费几分钟

 **设置 Composer 使用国内镜像**

执行命令[[设置 Composer 使用国内镜像](https://cloud.tencent.com/developer/labs/lab/10108#stage-2-step-2-mainland-composer)]：

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```



> 为了避免访问国外网络导致的延迟，推荐使用国内镜像源

 **使用 Composer 创建项目**

执行命令创建项目：

```
cd /var/www/html/ && composer create-project  showdoc/showdoc
```

 **设置 showdoc 目录写权限**

执行命令赋予 showdoc 下部分目录的写权限

```
chmod a+w showdoc/install
chmod a+w showdoc/Sqlite
chmod a+w showdoc/Sqlite/showdoc.db.php
chmod a+w showdoc/Public/Uploads/
chmod a+w showdoc/server/Application/Runtime
chmod a+w showdoc/server/Application/Common/Conf/config.php
chmod a+w showdoc/server/Application/Home/Conf/config.php
```

创建完毕，您现在可以通过浏览器访问 http://<您的 IP 地址>/showdoc/install/ ，进行语言的选择以后即可通过 http://<您的 IP 地址>/showdoc 查看站点效果。

## 3、准备域名和解析

 **域名注册**

注：如果您不需要通过域名访问您的站点，请通过`已完成，下一步`跳过`域名注册`环节

**域名解析**

注：如果您不需要通过域名访问您的站点，请通过`已完成，下一步`跳过`域名解析`环节

域名购买完成后, 需要将域名解析到实验云主机上。



域名设置解析后需要过一段时间才会生效，通过 `ping` 命令检查域名是否生效，如：

```
ping www.yourdomain.com
```

如果 ping 命令返回的信息中含有你设置的解析的 IP 地址，说明解析成功。



> 注意替换下面命令中的 `www.yourmpdomain.com` 为您自己的注册的域名

### 大功告成！

恭喜，您的 ShowDoc 站点已经部署完成，您可以通过浏览器访问查看效果。

通过IP地址查看：[http://<您的 IP 地址>/showdoc](http://xn--< cvm ip >-yp49ackh32qjw5g/showdoc)

通过域名查看：http://www.yourdomain.com/showdoc，其中替换 `www.yourdomain.com` 为之前申请的域名。

