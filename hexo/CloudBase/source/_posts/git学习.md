---
title: git学习
toc: true
tags:
  - git
  - 命令
abbrlink: e28ceec1
date: 2019-05-21 16:46:04
---

# Git 学习


![git](https://git-scm.com/images/logo@2x.png "git")


Git(读音为/gɪt/。)是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。


### 一、安装git

##### 1.在Linux上安装git

ubuntu上的命令：

```
$ sudo apt-get install git
```

如果是其他版本的Linux可以通过源码安装。

先从官网下载源码，然后解压，依次输入：./config , make , sudo make install这几个命令。
<!--more-->
##### 2.在Mac OS 上安装git

在Mac上安装推荐从APPSTORE安装。

##### 3.在Windows上安装git

1.  从git[官网下载程序](https://git-scm.com/downloads)安装，按默认一直点下一步就可以。

2.  安装完成后，在开始菜单或者右键里可以找到git/git bash here点击运行出现类似命令行窗口的东西，恭喜你git安装成功了！

3.  然后在命令行里配置你的用户名和email地址。

```
$ git config --global user.name "youname"
$ git config --global user.eamil "youemail" 
```


### 二、创建仓库

仓库就是版本库，英文名repository ，每个文件的修改、删除都在版本库里并且可以被追踪。

命令：
```
$ mkdir Learn   //创建文件夹
$ cd Learn      //进入文件夹Learn
$ git init      //初始化，把这个目录变成git仓库
```
这时文件夹里会出现一个.git目录，如果没有看到那就打开隐藏文件。

### 三、把文件添加到版本库

1.   首先我们建立一个文本文档命名为README.txt，用命令添加到仓库：

```
$ git add README.txt    //git add <file>可以多次使用，添加多个文件
```

2.   用命令git commit告诉git，把文件提交到仓库。-m后面的内容是本次提交的说明，可以是任意的内容：

```
$ git comnit -m "新建一个README.txt"
```

3.   可以使用命令git status 查看当前仓库的状态：

```
$ git status 
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   README.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

4.   使用命令git diff可以查看文件做了什么修改：

```
$ git diff README.txt
```
注：如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

5.   使用命令git log显示从最近到最远的提交日志：

```
$ git log   //如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数
$ git log  --pretty=oneline
```
6.   HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭

```
$ git reset --hard commit_id    //commit_id是版本号
```

7.   使用git reflog查看命令历史，以便确定要回到未来的哪个版本

```
$ git reflog 
```

8.   使用git checkout 命令可以丢弃工作区的修改

```
$ git checkout -- file
```

9.   使用git rm命令用于删除一个文件

```
$ git rm README.txt
```


### 四、把文件添加到远程仓库

1.  由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以第一步是创建SSH Key。

```
$ ssh-keygen -t rsa -C "youremail@example.com"  //一路回车，使用默认值即可

完成之后可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
```

2.   登录GitHub 打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。

3.   要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git

```
$ git remote add origin git@github.com:zhaozhifu-catshak/Learn.git
```

4.   关联后，使用命令git push -u origin master第一次推送master分支的所有内容，此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改
   

5.   用命令git clone克隆一个远程库到本地

```
$ git clone git@github.com:zhaozhifu-catshak/Learn.git
```

### 五、创建分支

1. 首先，我们创建read分支，然后切换到read分支：

```
$ git checkout -b read
Switched to a new branch 'read'
```

2. git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：

```
$ git branch read       //创建read分支
$ git checkout read     //切换到read分支
Switched to branch 'read'
```

3. 使用git branch命令查看当前分支：

```
$ git branch
* read
  master
```
4. git branch命令会列出所有分支，当前分支前面会标一个*号。

然后，我们就可以在read分支上正常提交。

5. 使用git merge命令用于合并指定分支到当前分支。

```
$ git merge read
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)

注：如果要合并到master主分支，要先切换到master主分支
```

6. 合并完成后，就可以放心地删除read分支了

```
$ git branch -d read
Deleted branch dev (was b17d20e).
```

7. 删除后，查看branch，就只剩下master分支了：

```
$ git branch
* master
```

8. 用git log --graph命令可以看到分支合并图.

9. 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
    
```
$ git merge --no-ff -m "merge with no-ff" read

//-m参数可以把commit描述写进去。
```
10. 使用git branch -D file 命令强行删除分支
11. 推送分支:

```
$ git push origin read
```

### 六、创建标签

1. 首先，切换到需要打标签的分支上，使用命令 git tag打标签。

```
$ git branch
* read
  master
$ git checkout master
Switched to branch 'master'
$ git tag v1.0
//$ git tag v0.9 commit id
//可以用commit id 给固定版本打标签
```

2. 用命令git tag查看所有标签.

```
$ git tag
//v0.9
v1.0
```

3. 命令git tag -a tagname -m "blablabla..."  可以指定标签信息
   
```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```
4. 使用参数-d删除 ***本地*** 标签

```
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)
```

5. 命令git push origin :refs/tags/tagname 可以删除一个远程标签

6. 命令git push origin tagname 可以推送一个本地标签；

7. 命令git push origin --tags可以推送全部未推送过的本地标签；
   
8. 让git显示颜色 

``` 
$ git config --global color.ui true
```










   - 2019年3月31日下午13：20



### 七、LFS（Large File Storage）

* 1.显示 Git LFS 系统环境 .
```
$ git lfs env:
```
* Populate working copy with real content from Git LFS files.
* 2.使用来自Git LFS文件的真实内容填充工作副本
```
$ git lfs checkout:
```
*     Download Git LFS files from a remote.
* 3.从远程下载Git LFS文件
```
$ git lfs fetch:
```
*    Check Git LFS files for consistency.
* 4.检查Git LFS文件的一致性
```
$ git lfs fsck:
```
*    Install Git LFS configuration.
* 5.安装Git LFS并配置
```
$ git lfs install:
```
*    Set a file as "locked" on the Git LFS server.
* 6.在Git LFS服务器上将文件设置为“锁定”。
```
$ git lfs lock:
```
*    List currently "locked" files from the Git LFS server.
* 7.列出当前从Git LFS服务器“锁定”的文件
```
$ git lfs locks:
```
*    Show errors from the Git LFS command.
* 8.显示来自Git LFS命令的错误。
```
$ git lfs logs:
```
*    Show information about Git LFS files in the index and working tree.
* 9.在索引和工作树中显示关于Git LFS文件的信息
```
$ git lfs ls-files:
```
*    Migrate history to or from Git LFS
* 10.将历史迁移到或从Git LFS
```
$ git lfs migrate:
```
*    Delete old Git LFS files from local storage
* 11.从本地存储中删除旧的Git LFS文件
```
$ git lfs prune:
```
*    Fetch Git LFS changes from the remote & checkout any required working tree files.
* 12.从远程获取Git LFS更改并检出任何所需的工作树文件
```
$ git lfs pull:
```
*    Push queued large files to the Git LFS endpoint.
* 13.将排队的大文件推送到Git LFS端点
```
$ git lfs push:
```
*    Show the status of Git LFS files in the working tree.
* 14.显示工作树中Git LFS文件的状态
```
$ git lfs status:
```
*    View or add Git LFS paths to Git attributes.
* 15.查看或向Git属性添加Git LFS路径
```
$ git lfs track:
```
*    Uninstall Git LFS by removing hooks and smudge/clean filter configuration.
* 16.通过删除挂钩和涂抹/清除过滤器配置卸载Git LFS
```
$ git lfs uninstall:
```
*    Remove "locked" setting for a file on the Git LFS server.
* 17.删除Git LFS服务器上文件的“锁定”设置。
```
$ git lfs unlock:
```
*    Remove Git LFS paths from Git Attributes.
* 18.从Git属性中删除Git LFS路径
```
$ git lfs untrack:
```
*  19.更新当前Git存储库的Git hooks.
```
$ git lfs update:
```
* 20.报告版本号.
```
$ git lfs version:
```


- Git LFS是一个与Git存储库相关联的大型文件管理和版本控制系统。Git LFS不将大型文件存储为blobs，而是在存储库中存储特殊的“指针文件”，同时将实际的文件内容存储在Git LFS服务器上。当需要时，会自动下载大文件的内容，例如签出包含大文件的Git分支时。
- Git LFS使用“smudge”过滤器根据指针文件查找大文件的内容，使用“clean”过滤器在大文件的内容发生更改时创建指针文件的新版本。它还使用预推钩子将包含新大文件版本的提交上传到相应的Git服务器时，将大文件内容上传到Git LFS服务器。


