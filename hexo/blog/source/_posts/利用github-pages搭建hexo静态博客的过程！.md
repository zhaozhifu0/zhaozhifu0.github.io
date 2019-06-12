---
title: 利用github pages搭建hexo静态博客过程！
date: 2019-05-22 12:35:22
tags:
    - 博客
    - Github Pages
categories:
    - Hexo
---

![](/assets/blogImg/linian.jpg)
# 一、注册github账号

1. 搜索Github官网进入，如果你已经有账号密码，那么点击右上角的sign in直接登录。
2. 如果没有点击sign up进行注册，填写昵称（用户名）、注册邮箱和密码。
3. 注册成功之后在Github首页右上角头像左侧加号点选择 New repositor(新存储库)进行创建一个代码仓库。
   注意：仓库的名字一定是 yourname.github.io 其他默认即可。

<!--more-->
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

```
新建完成后，指定文件夹目录下有：
- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- _config.yml: 博客的配置文件

```
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
$ hexo clean #清除之前生成的东西
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
> yilia 主题配置参考[官方文档](https://github.com/litten/hexo-theme-yilia)
> ***Hexo主题配置(根目录_config.yml文件)***

更多主题参看[hexo主题官网](https://hexo.io/themes/)

# 四、hexo插件
 1. 建立RSS订阅需要安装
```
$ npm install hexo-generator-feed --save
```
2.建立站点地图需要安装
```
$ npm install hexo-generator-sitemap --save
```
更多插件参考[插件官网](https://hexo.io/plugins/).

> 想要给网站添加图片？请把图片放入根目录 source\ 下建立一个文件夹，当你执行hexo g的时候此文件夹自动生成到public中。

# 五、部署hexo
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

# 六、配置域名
1. 购买域名
2. 域名注册提供商那里配置DNS解析， 参考[官方文档](https://help.github.com/en/articles/setting-up-an-apex-domain)
3. 到github page配置站点域名，参考 ：[Securing your GitHub Pages site with HTTPS](https://help.github.com/en/articles/securing-your-github-pages-site-with-https)
4. 最后可以用你的域名访问了。

# 七、跳过配置显示demo
1. 使用Hexo提供的跳过渲染配置，适用于整个目录的设置。
2. 打开博客根目录_config.yml，找到其中skip_render项，这个项目用来配置/source/中需要跳过渲染的文件或目录，例如希望跳过/source/photowall/里的所有文件渲染，可以配置为：

```
skip_render: photowall/**
```
3. 给单个文件添加不应用模板的标记，适用于个别特殊文件的处理。例如我们的网站如果要使用百度统计，往往需要在根目录放一个html格式的验证文件，这个文件默认也会经过用主题模板渲染，避免渲染的办法就是在文件头部添加如下内容：

```
---
layout: false     #添加layout跳过编译
title: 利用github pages搭建hexo静态博客过程！
date: 2019-05-22 12:35:22
tags:
    - 博客
    - Github Pages
---
```

# 参考文献
> [使用hexo，如果换了电脑怎么更新博客？](https://www.zhihu.com/question/21193762)
> [Markdown——入门指南](https://www.jianshu.com/p/1e402922ee32/)
> [手把手教你使用Hexo + Github Pages搭建个人独立博客](https://segmentfault.com/a/1190000004947261)
> [Hexo搭建GitHub博客—打造炫酷的NexT主题--高级(三)](https://eirunye.github.io/2018/09/02/Hexo%E6%90%AD%E5%BB%BAGitHub%E5%8D%9A%E5%AE%A2%E2%80%94%E6%89%93%E9%80%A0%E7%82%AB%E9%85%B7%E7%9A%84Next%E4%B8%BB%E9%A2%98%E2%80%94%E9%AB%98%E7%BA%A7%E2%80%94%E4%B8%89/#more)
> [我是如何利用Github Pages搭建起我的博客，细数一路的坑](https://www.cnblogs.com/jackyroc/p/7681938.html)
> [手把手教你使用Hexo + Github Pages搭建个人独立博客](https://segmentfault.com/a/1190000004947261)

# 其他扩展
[为什么我选择用 Github issues 来写博客](https://juejin.im/post/5ce53de85188252d46797fee)