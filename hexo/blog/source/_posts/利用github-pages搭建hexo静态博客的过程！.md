---
title: 利用github pages搭建hexo静态博客过程！
toc: true
tags:
  - 博客
  - Github Pages
categories:
  - Hexo
abbrlink: 6944216c
date: 2019-05-22 12:35:22
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
1.1 在博客工程文件根目录下_config.yml文件中添加如下内容
```
# Extensions
plugins:
    hexo-generator-feed
#Feed Atom
feed:
    type: atom
    path: atom.xml
    limit: 20
```
1.2 打开主题配置文件_config.yml,搜索rss，添加配置
```
$ rss: /atom.xml
```
```
$ hexo g
$ hexo s
```
1.3 之后浏览器预览就可以了。
 2. 建立站点地图需要安装
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
6. 部署到coding上：

(1)在coding上创建一个项目，进入项目里“代码”页面，点击“一键开启静态 Pages”，然后上传项目代码。
(2)在上传之前打开 Hexo 博客主配置文件 _config.yml，找到 deploy 属性，作如下配置：
```
deploy:
  type: git
  repo: 
      github: git@github.com:zhaozhifu0/zhaozhifu0.github.io.git
      coding: git@git.dev.tencent.com:zhaozhifu/zhaozhifu.git
  branch: master
```
(3)上传之后就可以通过<user_name>.coding.me 访问你的网站。
(4)绑定自定义域名:
> Coding Pages 支持绑定自定义域名，域名理论上不用备案，但是根据国家规定建议进行ICP备案。允许以下自定义域名格式：
> 顶级域名 example.com
> www 二级域名 www.example.com
> 二级域名 blog.example.com

(5)进入域名服务商提供的域名管理面板，选中欲绑定的域名，进入解析设置，点击"添加解析"。
(6)添加 CNAME 记录，在项目设置中绑定域名下面有行小字 ： 绑定前请在域名 DNS 设置中添加一条 CNAME 记录指向 xxxx.coding.me。将@和www记录都解析到这个即可。
(7)在"Pages 服务"设置页中绑定自定义域名，如果配置 DNS 解析时域名前缀为 ，绑定自定义域名需要填写 www 前缀。
(8)如果绑定后访问域名看到404相关提示，说明是域名配置是存在问题的，请自行检查DNS配置是否正确。


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
# 八、其他

## 文章如何显示摘要
点击主页时，发现所有文章都是全文显示，不利于查找，可控制显示的字数。
解决办法：在你 MD 格式文章正文插入 <!-- more -->即可，只会显示它之前的，此后的就不显示，点击文章标题，全文阅读才可看到，同时注释掉以下 themes/yilia/_config.yml，重复

```
# excerpt_link: more
```

## 文章显示目录
增加文章目录 TOC(table of content )，方便阅读文章, 在 themes/yilia/_config.ym中进行配置 toc: 2即可，它会将你 Markdown 语法的标题，生成目录，目录查看在右下角。

## 增加不蒜子统计
普通用户只需两步走：一行脚本+一行标签，搞定一切。追求极致的用户可以进行任意DIY。

#####  一、安装脚本（必选）
要使用不蒜子必须在页面中引入busuanzi.js，目前最新版如下。
```
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
</script>
```
不蒜子可以给任何类型的个人站点使用，如果你是用的hexo，打开themes/你的主题/layout/_partial/footer.ejs添加上述脚本即可，当然你也可以添加到 header 中。

##### 二、安装标签（可选）
只需要复制相应的html标签到你的网站要显示访问量的位置即可。您可以随意更改不蒜子标签为自己喜欢的显示效果，内容参考第三部分扩展开发。根据你要显示内容的不同，这分几种情况。

- 1、显示站点总访问量
要显示站点总访问量，复制以下代码添加到你需要显示的位置。有两种算法可选：

算法a：pv的方式，单个用户连续点击n篇文章，记录n次访问量。
```
<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>
```
算法b：uv的方式，单个用户连续点击n篇文章，只记录1次访客数。
```
<span id="busuanzi_container_site_uv">
  本站访客数<span id="busuanzi_value_site_uv"></span>人次
</span>
```
如果你是用的hexo，打开themes/你的主题/layout/_partial/footer.ejs添加即可。

## 插入网易云音乐
登入网易云音乐网页版，选择一首歌，点击歌曲详情，点击生成外链播放器,复制外链代码，插入你需要编辑的 MD 格式文章里面，即可.
![](/assets/blogImg/201906261615.png)

## Url链接优化
我们可以发现hexo默认生成的文章地址路径是 【网站名称／年／月／日／文章名称】。

这种链接对搜索爬虫是很不友好的，它的url结构超过了三层，太深了。

下面我推荐安装hexo-abbrlink插件：
```
npm install hexo-abbrlink --save
```
- 实现思路：
对标题+时间进行md5然后再转base64，保存在front-matter中。

然后配置_config.yml
```
# permalink: :title/
permalink: archives/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

## Gitment评论

gitment其实就是利用你的代码仓库的Issues，来实现评论。每一篇文章对应该代码仓库中的一个Issues,Issues中的评论对应你的博客每篇文章中的评论。

1. 注册 OAuth Application
   首先要注册一个OAuth Application应用可以点击[这里](https://github.com/settings/applications/new)。 输入的内容如下：
   Application name （随意就好，可以写你博客名称）
   Homepage URL     （你的博客地址，例如https://zhaozhifu08.github.io/ 如果有域名可以填写域名https://zhaozf.site  ）
   Application description (随意就好)
   Authorization callback URL (也是博客地址，例如https://zhaozhifu08.github.io/ 如果有域名可以填写域名https://zhaozf.site)
   输入完成之后，点击注册就OK了。成功之后就会拿到Client ID和Client Secret，然后先进行一下步，记住这两个后面步骤会用到。
2. 修改_config.yml文件
   在themes->yilia->_config.yml中修改如下：
```
#5、Gitment
gitment_owner: zhaozhifu0     #你的 GitHub ID
gitment_repo: 'zhaozhifu0.github.io'          #存储评论的 repo
gitment_oauth:
  client_id: '41be2b66b56556bc6c7c'           #client ID
  client_secret: '3338513189c56c3740d424cd2268be4a4923d99a'       #client secret
```
3. 查看结果
运行命令：hexo s到浏览器查看结果 https://localhost:4000

4. 遇到的坑

4.1 Error: Not Found
- owner或repo配置错误
- owner建议直接写github帐号
- repo填写存放评论的仓库，填写仓库名称就可以了，不需要加https://github.com/xxx/

4.2 Error: Comments Not Initialized
- 在该页面的Gitment评论区登陆GitHub账号
- 之前OAuth Application callback填写有误

4.3  Error：validation failed

- id太长
- issue的标签label有长度限制是50个字符
- 建议设置为发布博客的日期+时间：
  修改themes\yilia\layout\_partial\post\gitment.ejs 文件

```
var gitment = new Gitment({
  id: "<%=url%>",                   //修改为 id: "<%=page.date%>",
  owner: '<%=theme.gitment_owner%>',
  repo: '<%=theme.gitment_repo%>',
  oauth: {
    client_id: '<%=theme.gitment_oauth.client_id%>',
    client_secret: '<%=theme.gitment_oauth.client_secret%>',
  },
})
gitment.render('gitment-ctn')
```

5. Gitment 汉化
- 修改themes\yilia\layout\_partial\post\gitment.ejs 文件
```
<link rel="stylesheet" href="//imsun.github.io/gitment/style/default.css">
<script src="//imsun.github.io/gitment/dist/gitment.browser.js"></script> 
```
将原来的修改为如下：
```
<link rel="stylesheet" href="https://billts.site/extra_css/gitment.css">
<script src="https://billts.site/js/gitment.js"></script>
```

## 实现私密文件加密

- 安装插件命令`npm install --save hexo-blog-encrypt`或 `yarn add hexo-blog-encrypt`(需要) [Yarn](https://classic.yarnpkg.com/en/)，然后编辑`_config.yml`文件，添加如下内容：
```
# Security
## 文章加密 hexo-blog-encrypt
encrypt:
    enable: true
```

- 然后在想加密的文章头部添加上对应字段，如下：
```
---
title: hello world
date: 2016-03-30 21:18:02   
tags:       
password: 12345   （密码）
abstract: 查看文章，请输入文章密码。 
message: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你确实想看，请微信搜索公众号名称【无聊才读书呀】 或微信号：【zhaozfsite】回复关键字【博客密码】，获取文章密码。  
---
```
- password: 是该博客加密使用的密码
- abstract: 是该博客的摘要，会显示在博客的列表页
- message: 这个是博客查看时，密码输入框上面的描述性文字

详细的内容和配置请参考github[hexo-blog-encrypt](https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md)

[其他特效参考](http://ds666.fun/2019/01/25/%E6%90%AD%E5%8D%9A%E5%AE%A2%E5%85%A8%E8%BF%87%E7%A8%8B%E5%8F%8A%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%E5%92%8C%E7%BE%8E%E5%8C%96/)
[点击出现文字效果](https://xhyeax.github.io/2018/11/30/click-word/)
[下雪特效](https://blog.csdn.net/stormdony/article/details/86001618)

# 参考文献
> [使用hexo，如果换了电脑怎么更新博客？](https://www.zhihu.com/question/21193762)
> [Markdown——入门指南](https://www.jianshu.com/p/1e402922ee32/)
> [手把手教你使用Hexo + Github Pages搭建个人独立博客](https://segmentfault.com/a/1190000004947261)
> [Hexo搭建GitHub博客—打造炫酷的NexT主题--高级(三)](https://eirunye.github.io/2018/09/02/Hexo%E6%90%AD%E5%BB%BAGitHub%E5%8D%9A%E5%AE%A2%E2%80%94%E6%89%93%E9%80%A0%E7%82%AB%E9%85%B7%E7%9A%84Next%E4%B8%BB%E9%A2%98%E2%80%94%E9%AB%98%E7%BA%A7%E2%80%94%E4%B8%89/#more)
> [我是如何利用Github Pages搭建起我的博客，细数一路的坑](https://www.cnblogs.com/jackyroc/p/7681938.html)
> [手把手教你使用Hexo + Github Pages搭建个人独立博客](https://segmentfault.com/a/1190000004947261)
> [hexo的next主题个性化教程:打造炫酷网站](https://www.jianshu.com/p/f054333ac9e6)
> [自己动手修改完善yilia主题](https://www.codercto.com/a/55846.html)
> [自己动手修改完善yilia主题（下）](https://www.codercto.com/a/57534.html)

# 其他扩展
[为什么我选择用 Github issues 来写博客](https://juejin.im/post/5ce53de85188252d46797fee)
[修改Yilia左下角SubNav的社交图标](https://blog.zscself.com/posts/70677220/)