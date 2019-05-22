---
title: 利用github pages搭建hexo静态博客过程！
date: 2019-05-22 12:35:22
tags:
    - 博客
    - Github Pages
---

![](/assets/blogImg/linian.jpg)
# 一、注册github账号

1. 搜索Github官网进入，如果你已经有账号密码，那么点击右上角的sign in直接登录。
2. 如果没有点击sign up进行注册，填写昵称（用户名）、注册邮箱和密码。
3. 注册成功之后在Github首页右上角头像左侧加号点选择 New repositor(新存储库)进行创建一个代码仓库。
   注意：仓库的名字一定是 yourname.github.io 其他默认即可。
4. 创建完成之后，点击Settings查看github pages是否启用
   ![](/assets/blogImg/20190522125619.png)
5. 之后就可以通过 https://yourname.github.io 进行访问了。

# 二、搭建hexo静态博客系统

1. hexo需要node.js的支持，首先[下载](https://nodejs.org/en/)node.js的安装包。node.js的安装包下载完成之后，一路下一步就可以了。检测node.js是否正确安装可以进入cmd命令行输入 node --version 查看版本。
2. [下载git](https://git-scm.com/download/) 也是一直下一步就可以了。git的用法参见廖雪峰的[git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
3. 在你需要安装Hexo的目录下右键选择 Git Bash
```
$ cd d:/hexo
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo g # 或者hexo generate 是生成静态文件，会在当前目录下生成一个新的叫做public的文件夹
$ hexo s # 或者hexo server，可以在http://localhost:4000/ 查看。  hexo server (hexo s) 是启动本地web服务，用于博客的预览
```
另外还有其他几个常用命令：
```
$ hexo new "postName" #新建文章
$ hexo new page "pageName" #新建页面
$ hexo d -g #生成部署
$ hexo s -g #生成预览
```
现在我们打开http://localhost:4000/ 已经可以看到一篇内置的blog了。

# 三、hexo主题
1. 我这里用的主题是yilia，安装完成的默认主题是landscape.    
```
$ hexo clean
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```
2. 修改Hexo目录下的_config.yml配置文件中的theme属性，将其设置为yilia。
3. 更新主题
```
$ cd themes/yilia
$ git pull
$ hexo g # 生成
$ hexo s # 启动本地web服务器
```
打开http://localhost:4000/ ，你会看到我们一个新的主题。
yilia 主题配置参考[官方文档](https://github.com/litten/hexo-theme-yilia)

# 四、部署hexo
1. 使用命令hexo deploy进行部署。它可以部署很多平台，具体请参考[官方文档](https://hexo.io/docs/deployment.html) 。我这里以部署到git为例：
2. 安装 hexo-deployer-git.
```
$ npm install hexo-deployer-git --save
```
3. 编辑 _config.yml 文件:
```
deploy:
  type: git
  repo: git@github.com:zhaozhifu0/zhaozhifu0.github.io.git  #这里改成你自己的
  branch: master  
```
4. 执行hexo deploy命令进行上传部署。
```
$ hexo deploy #或者hexo d(hexo deploy的简写) 再或者hexo d -g (生成部署)
```
5. 最后访问https://yourname.github.io 就可以查看博客的内容了。还可以通过hexo new "postName" 命令创建新文章。