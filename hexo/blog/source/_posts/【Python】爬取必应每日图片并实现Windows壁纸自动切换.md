---
title: 【Python】爬取必应每日图片并实现Windows壁纸自动切换
tags:
  - 爬取必应每日一图
  - bing 必应
  - Windows壁纸
categories:
  - Python
abbrlink: f48b09c5
date: 2021-03-31 22:15:33
---



![](/assets/blogImg/202103312233.jpg)

下面用python脚本抓取必应每日图片，并实现桌面壁纸的每天自动切换。

<!--more-->

**思路整理** 

**
**

1、通过网页，获取图片地址

2、保存图片到绝对路径

3、设置该绝对路径所指向的图片为壁纸

4、批处理壁纸自动切换



需要用到的模块如下：



```
import urllib.request
import requests
import os.path
import ctypes
```



**第一、**

**获取图片地址** 



这个函数主要通过requests模块，根据必应的网页地址，获取到当日图片的最终img地址。



```
# 请求网页，跳转到最终 img 地址
def get_img_url(raw_img_url="https://area.sinaapp.com/bingImg/"):
    r = requests.get(raw_img_url)
    img_url = r.url  # 得到图片文件的网址
    print('img_url:', img_url)
    return img_url
```





**第二、**

**保存图片到本地** 



这个函数的作用就是把图片保存到你自己设置的一个目录下，并返回当前目录的绝对地址。



```
def save_img(img_url, dirname):
    # 保存图片到磁盘文件夹dirname中
    try:
        if not os.path.exists(dirname):
            print('文件夹', dirname, '不存在，重新建立')
            # os.mkdir(dirname)
            os.makedirs(dirname)
        # 获得图片文件名，包括后缀
        basename = "bing.jpg"
        # 拼接目录与文件名，得到图片路径
        filepath = os.path.join(dirname, basename)
        # 下载图片，并保存到文件夹中
        urllib.request.urlretrieve(img_url, filepath)
    except IOError as e:
        print('文件操作失败', e)
    except Exception as e:
        print('错误 ：', e)
    print("Save", filepath, "successfully!")

    return filepath
```



**第三、**

**设置该绝对路径所指向的图片为壁纸** 



通过之前获得的图片所在的绝对路径，把该图片设置为桌面壁纸。



```
def set_img_as_wallpaper(filepath):
    ctypes.windll.user32.SystemParametersInfoW(20, 0, filepath, 0)
```



**第四、
**

**运行代码的main函数** 

**
**

```
def main():
    dirname = "D:\\bingImg"  # 图片要被保存在的位置
    img_url = get_img_url()
    filepath = save_img(img_url, dirname)  # 图片文件的路径
    set_img_as_wallpaper(filepath)
```



运行



**第五、
**

**批处理自动更换壁纸** 



此时，可以在python脚本的同一目录下创建名为py_bingying.bat的批处理文件，批处理内容如下：



```
@echo off
del g:\bingImg\*.jpg
python SetBingImgAsWallpaper.py
```



第二行在运行python脚本前先删除前一天下载的必应图片，这样就实现了旧壁纸的每日清理，最大限度节省了存储空间。第三行为运行上面的python脚本。



如何实现壁纸的自动切换呢，这里采用开机运行上面的批处理程序的方法。


复制上面创建的批处理文件，到下方目录下，右键-粘贴为快捷方式。这样就实现了开机启动批处理程序，自动清除和更新壁纸。



C:\User\yourname\AppData\Roaming\Microsoft\Windows\开始菜单\程序\启动



每次开机都执行一遍更换壁纸的操作还不够完美的话，可以用Windows任务计划程序来添加任务，设置每天指定时间点运行批处理程序。



完整代码：

```
#!/usr/bin/env python

\# -*- coding: utf-8 -*-





import urllib.request

import requests

import os.path

import ctypes





def save_img(img_url, dirname):

  \# 保存图片到磁盘文件夹dirname中

  try:

​    if not os.path.exists(dirname):

​      print('文件夹', dirname, '不存在，重新建立')

​      \# os.mkdir(dirname)

​      os.makedirs(dirname)

​    \# 获得图片文件名，包括后缀

​    basename = "bing.jpg"

​    \# 拼接目录与文件名，得到图片路径

​    filepath = os.path.join(dirname, basename)

​    \# 下载图片，并保存到文件夹中

​    urllib.request.urlretrieve(img_url, filepath)

  except IOError as e:

​    print('文件操作失败', e)

  except Exception as e:

​    print('错误 ：', e)

  print("Save", filepath, "successfully!")



  return filepath





\# 请求网页，跳转到最终 img 地址

def get_img_url(raw_img_url="https://area.sinaapp.com/bingImg/"):

  r = requests.get(raw_img_url)

  img_url = r.url # 得到图片文件的网址

  print('img_url:', img_url)

  return img_url





\# 设置图片绝对路径 filepath 所指向的图片为壁纸

def set_img_as_wallpaper(filepath):

  ctypes.windll.user32.SystemParametersInfoW(20, 0, filepath, 0)





def main():

  dirname = "D:\\bingImg" # 图片要被保存在的位置

  img_url = get_img_url()

  filepath = save_img(img_url, dirname) # 图片文件的路径

  set_img_as_wallpaper(filepath)





if __name__ == '__main__':

  main()
```

