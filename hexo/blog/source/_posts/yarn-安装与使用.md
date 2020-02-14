---
title: yarn 安装与使用
toc: true
tags:
  - yarn
categories:
  - 包管理器
abbrlink: 2d1e74c1
date: 2020-02-04 12:01:00
---

![yarn包管理器](/assets/blogImg/20200204.png)

# 1 - Introduction

Yarn 对你的代码来说是一个包管理器， 你可以通过它使用全世界开发者的代码， 或者分享自己的代码。yarn可以快速、安全、可靠地做到这一点，所以你永远不必担心。

通过Yarn你可以使用其他开发者针对不同问题的解决方案，使自己的开发过程更简单。

代码通过 包（package） (或者称为 模块（module）) 的方式来共享。 一个包里包含所有需要共享的代码，以及描述包信息的文件，称为 package.json 。

<!--more-->
# 2 - Installation

注意：当前，Yarn 2仅可通过npm安装。以前在Yarn 1中可用的安装方法（Windows安装程序，Chocolatey，Debian / Ubuntu软件包，Homebrew和RPM软件包）将很快再次可用。

1.安装Node.js

2.安装yarn

`npm install -g yarn@berry`

3.通过运行以下命令测试Yarn 2是否已正确安装，其结果应为“ v2.0.0”或类似的结果：

`yarn --version`

## 每个项目的安装

1.遵循全局安装说明

2.移至您的项目文件夹：

`cd ~/path/to/project`

3.运行以下命令：

`yarn set version berry`

4.提交.yarn和.yarnrc.yml更改

# 3 - Usage

现在，您已经安装了 Yarn ，可以开始使用Yarn了。这是您需要的一些最常见的命令。

访问命令列表

`yarn help`

开始一个新项目

`yarn init`

安装所有依赖项

yarn
`yarn install`

添加依赖

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

将依赖项添加到不同类别的依赖项中
```
yarn add [package] --dev  # dev dependencies
yarn add [package] --peer # peer dependencies
```

升级依赖

yarn up [package]
yarn up [package]@[version]
yarn up [package]@[tag]

删除依赖

`yarn remove [package]`

升级yarn本身

`yarn set version 2.0.0`
`yarn set version from sources`