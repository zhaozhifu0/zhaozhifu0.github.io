---
title: uni-app 跨平台框架学习笔记【一】
toc: true
tags:
  - uni-app
  - 微信小程序
categories:
  - 跨平台框架
abbrlink: 629227f3
date: 2020-01-14 09:43:22
---
![uni-app学习笔记](/assets/blogImg/201001141036.jpg)

uni-app 是一个使用 Vue.js 开发所有前端应用的框架，

开发者编写一套代码，可发布到iOS、Android、H5、

以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉）等多个平台。

uni-app 使用vue的语法+小程序的标签和API。功能架构如下图：
<!--more-->

![uni-app功能框架图](/assets/blogImg/202001141040.png)

## uni-app 约定了如下开发规范：

- 页面文件遵循 Vue [单文件组件 (SFC) 规范](https://vue-loader.vuejs.org/zh/spec.html#%E7%AE%80%E4%BB%8B)
- 组件标签靠近小程序规范，详见[官网uni-app 组件规范](https://uniapp.dcloud.io/component/README)
- 接口能力（JS API）靠近微信小程序规范，但需将前缀 wx 替换为 uni，详见uni-app接口规范
- 数据绑定及事件处理同 Vue.js 规范，同时补充了App及页面的生命周期
- 为兼容多端运行，建议使用flex布局进行开发

## 目录结构

一个uni-app工程，默认包含如下目录及文件：

┌─components            uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                存放本地网页的目录
├─platforms             存放各平台专用页面的目录
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─wxcomponents          存放小程序组件的目录
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息
└─pages.json            配置页面路由、导航条、选项卡等页面类信息

注意：
    static 目录下的 js 文件不会被编译，如果里面有 es6 的代码，不经过转换直接运行，在手机设备上会报错。
    css、less/scss 等资源同样不要放在 static 目录下，建议这些公用的资源放在 common 目录下。

## 生命周期

### 应用生命周期

函数名|说明
---|:--:
onLaunch|当uni-app 初始化完成时触发（全局只触发一次）
onShow|当 uni-app 启动，或从后台进入前台显示
onHide|当 uni-app 从前台进入后台
onError|当 uni-app 报错时触发
onUniNViewMessage|对 nvue 页面发送的数据进行监听，可参考 nvue 向 vue 通讯


### 页面生命周期

- onLoad	监听页面加载，其参数为上个页面传递的数据，参数类型为Object（用于页面传参）
- onShow	监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面		
- onReady	监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发		
- onHide	监听页面隐藏		
- onUnload	监听页面卸载		
- onResize	监听窗口尺寸变化		
- onPullDownRefresh	监听用户下拉动作，一般用于下拉刷新
- onReachBottom	页面上拉触底事件的处理函数		
- onTabItemTap	点击 tab 时触发，参数为Object，具体见官方文档		
- onShareAppMessage	用户点击右上角分享		
- onPageScroll	监听页面滚动，参数为Object		
- onNavigationBarButtonTap	监听原生标题栏按钮点击事件，参数为Object	
- onBackPress	监听页面返回，返回 event = {from:backbutton、 navigateBack} ，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是 uni.navigateBack ；详细说明及使用：onBackPress 详解	
- onNavigationBarSearchInputChanged	监听原生标题栏搜索输入框输入内容变化事件	
- onNavigationBarSearchInputConfirmed	监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发。	
- onNavigationBarSearchInputClicked	监听原生标题栏搜索输入框点击事件

## 路由

uni-app页面路由为框架统一管理，开发者需要在pages.json里配置每个路由页面的路径及页面样式。类似小程序在app.json中配置页面路由一样。所以 uni-app 的路由用法与 Vue Router 不同，如仍希望采用 Vue Router 方式管理路由，可在插件市场搜索 Vue-Router。

### 路由跳转
uni-app 有两种页面路由跳转方式：使用navigator组件跳转、调用API跳转。

## 运行环境判断
开发环境和生产环境
uni-app 可通过 process.env.NODE_ENV 判断当前环境是开发环境还是生产环境。一般用于连接测试服务器或生产服务器的动态切换。

cli模式下，是通行的编译环境处理方式。
```
if(process.env.NODE_ENV === 'development'){
    console.log('开发环境')
}else{
    console.log('生产环境')
}
```
## 页面样式与布局

