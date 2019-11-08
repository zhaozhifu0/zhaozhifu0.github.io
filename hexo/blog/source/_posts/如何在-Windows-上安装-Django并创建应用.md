---
title: 如何在 Windows 上安装 Django并创建应用
tags:
  - python
  - Django
categories:
  - Python
abbrlink: 7b73b3e5
date: 2019-10-17 18:03:31
---

![](/assets/blogImg/201910251755.jfif)

> 本文将指导您如何在 Windows 上安装 Python 3.7.4和 Django。 
> 它还提供了关于安装 virtualenv 和 virtualenvwrapper 的说明。
> 这使得在 Python 项目中工作更加容易。  
> 这是为 Django 项目工作的用户提供的初学者指南，并不反映在为 Django 本身开发补丁时应该如何安装 Django。


本指南中的步骤已经在 Windows 7、8和10中测试过了。 在其他版本中，步骤类似。 你将需要熟悉使用命令提示符。

<!--more-->
## 安装 Python

Django 是一个 Python web 框架，因此需要在机器上安装 Python。 在撰写本文时，使用的是Python 3.7.4版本。

要在你的机器上安装 Python，请访问 google https://Python.org/downloads/ 。 这个网站应该为你提供一个最新版本的 Python 下载按钮。 下载可执行安装程序并运行它。 选中“向 PATH 添加 Python 3.5”旁边的复选框，然后单击“立即安装”。

安装完成后，打开命令提示符并检查 Python 版本是否与您通过执行以下命令安装的版本相匹配:

```
python --version
```


## 关于pip

Pip 是 Python 的一个包管理。 它进行安装和卸载 Python 包(比如 Django!) 很简单。 对于安装的其余部分，我们将使用 pip 从命令行安装 Python 包。

要在您的计算机上安装 pip，请转到安装 https://pip.pypa.io/en/latest/Installing/ ，并按照 get-pip.py 说明进行安装。

如果您正在使用 Python 22.7.9或 Python 33.4，或者您正在 virtualenv 或 pyvenv 创建的虚拟环境中工作，那么 pip 已经安装。 只需确保升级 pip 即可。



## 安装virtualenv 及virtualenvwrapper

Virtualenv 和 virtualenvwrapper 为您创建的每个 Django 项目提供一个专用的环境。 虽然不是强制性的，但这被认为是最佳实践，并且在将来准备部署项目时将为您节省时间。 简单地输入:
```
pip install virtualenvwrapper-win
```
然后为你的项目创建一个虚拟环境:
```
mkvirtualenv myproject
```
虚拟环境将被自动激活，您将在命令提示符旁边看到“(myproject)”来指定该虚拟环境。 如果你启动一个新的命令提示符，你需要使用以下命令再次激活环境:
```
workon myproject
```

## 安装 Django

可以在虚拟环境中使用 pip 轻松地安装 Django。

在命令提示符中，确保虚拟环境处于活动状态，并执行以下命令:
```
pip install django
```
这将下载并安装最新的 Django 版本。

安装完成后，您可以通过在命令提示符中执行 Django-admin -- version 来验证您的 Django 安装。

## 编写你的第一个 Django 应用程序

#### 创建一个项目

如果这是您第一次使用 Django，那么您必须处理一些初始设置。 也就是说，您需要自动生成一些代码来建立 Django 项目—— Django 实例的设置集合，包括数据库配置、特定于 Django 的选项和特定于应用程序的设置。

从命令行中，cd 进入你想存储代码的目录，然后运行以下命令:
```
...\> django-admin startproject mysite
```
这将在你的工作目录文件夹中创建一个 mysite 目录。 如果不起作用，请参阅运行 django-admin 的问题。

让我们来看看 startproject 创建了什么:
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```


- manage.py：命令行实用程序，允许您以各种方式与这个 django 项目交互。
- mysite/__init__.py: 一个空文件，它告诉 Python 这个目录应该被视为一个 Python 包。
- mysite/settings.py: 此 Djangoproject 的设置 / 配置。
- mysite/urls.py: 这个 Django 项目的 URL 声明; 您的 Django 驱动站点的“目录”。
- mysite/wsgi.py:为您的项目提供与 wsgi 兼容的 web 服务器的入口点。

## 开发服务器

让我们验证一下您的 Django 项目是否正常工作。 如果你还没有进入外部 mysite 目录，可以运行以下命令:
```
...\> py manage.py runserver
```
您将在命令行上看到以下输出:
```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

October 16, 2019 - 15:50:53
Django version 2.2, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
您已经启动了 Django 开发服务器，这是一个纯粹用 Python 编写的轻量级 Web 服务器。 我们已经在 Django 中包含了这一点，这样您就可以快速开发，而无需配置生产服务器(比如 Apache) ，直到您准备好投入生产。

现在是注意的好时机: 不要在任何类似于生产环境的环境中使用此服务器。 它只能在开发过程中使用。 (我们的业务是制作 Web 框架，而不是 Web 服务器。)

现在服务器已经运行，可以使用 Web 浏览器访问 google http://127.0.0.1:8000/ 。 你会看到一个“恭喜! ” 佩奇，火箭正在起飞。 成功了！

![](/assets/blogImg/201910171811.png)

#### ①改变端口
默认情况下，runserver 命令在内部 IP 端口8000启动开发服务器。
如果要更改服务器端口，请将其作为命令行参数传递。 例如，这个命令在端口8080上启动服务器:
```
...\> py manage.py runserver 8080
```

如果您想更改服务器的 IP，请将它与端口一起传递。 例如，要侦听所有可用的公共 ip (如果你正在运行 Vagrant 或者想在网络上的其他计算机上炫耀你的工作，这是很有用的) ，使用:
```
...\> py manage.py runserver 0:8000
```
0是0.0.0.0的捷径。 开发服务器的完整文档可以在 runserver 参考中找到。

#### ②运行服务器的自动重新加载

开发服务器根据需要为每个请求自动重新加载 Python 代码。 您不需要重新启动服务器，代码更改即可生效。 但是，像添加文件样的
操作不会触发重新启动，因此在这些情况下必须重新启动服务器。

## 创建投票应用程序

现在您的环境(一个“项目”)已经设置好了，您就可以开始工作了。

您在 Django 中编写的每个应用程序都由遵循某种约定的 Python 包组成。 自带了一个工具，可以自动生成应用程序的基本目录结构，所以你可以专注于编写代码而不是创建目录。

>  ## 项目 vs 应用程序
> 项目和应用程序有什么区别？ 应用程序是一个 Web 应用程序，它可以做一些事情，比如 Weblog 系统、公共记录数据库或者一个简单的投票应用程序。 项目是针对特定网站的配置和应用程序的集合。 一个项目可以包含多个应用程序。 一个应用程序可以在多个项目中。


你的应用程序可以在你的 Python 路径上的任何地方运行。 在本教程中，我们将在 manage.py 文件旁边创建我们的投票应用程序，这样它就可以作为自己的顶级模块导入，而不是 mysite 的子模块。

要创建你的应用程序，确保你和 manage.py 在同一个目录下，然后输入以下命令:
```
...\> py manage.py startapp polls
```

这将创建一个目录民意调查，如下所示:
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```
这个目录结构将存放轮询应用程序。

## 写下你的第一个观点

让我们编写第一个视图。 打开文件 polls / views.py 并在其中放入以下 Python 代码:

> polls/views.py
```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")

```
这是 Django 中最简单的视图。 要调用这个视图，我们需要将它映射到一个 URL ——为此我们需要一个 URLconf。

要在 polls 目录中创建 URLconf，请创建一个名为 urls.py 的文件。 你的应用程序目录现在应该是这样的:
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```
在 polls / urls.py 文件中包含以下代码:
> polls/urls.py
```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```
下一步是将根 URLconf 指向 polls.url 模块。 在 mysite / urls.py 中，为 django.urls.include 添加一个导入，并在 urlpatterns 列表中插入一个 include () ，这样就可以:

> mysite/urls.py

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```
函数允许引用其他 URLconfs。 无论 Django 遇到什么 include () ，它都会删除与该点匹配的 URL 的任何部分，并将剩余的字符串发送到包含的 URLconf 进行进一步处理。

Include ()背后的思想是为了便于插入和播放 url。 因为民意调查是在它们自己的 URLconf (polls / urls.py)中，所以它们可以放在“ / polls / ” ，或者“ / fun polls / ” ，或者“ / content / polls / ” ，或者任何其他路径根目录下，这个应用程序仍然可以工作。


> #### 何时使用 include ()
> 当您包含其他 URL 模式时，您应该始终使用 include ()。 Url 是唯一的例外。

您现在已经将索引视图连接到 URLconf。 确认它正在使用以下命令:

```
...\> py manage.py runserver
```
在你的浏览器中进入 http://localhost:8000/polls/ ，你会看到文本“Hello, world. You’re at the polls index.” ，您在索引视图中定义的。

> #### 页面没有找到？
> 如果你在这里得到一个错误页面，检查你要去的是 http://localhost:8000/polls/ ，而不是 http://localhost:8000/ 。


Path ()函数传递了四个参数，其中两个是必需的: route 和 view，两个是可选的: kwargs 和 name。 在这一点上，我们有必要回顾一下这些争论的目的。


### path() argument: route

Route 是一个包含 URL 模式的字符串。 当处理一个请求时，Django 从 urlpatterns 中的第一个模式开始，沿着列表一路向下，将请求的 URL 与每个模式进行比较，直到找到匹配的模式。

模式不搜索 GET 和 POST 参数或域名。 例如，在对 myapp / https://www.example.com/myapp/ 的请求中，URLconf 将查找 myapp / 。 在 https://www.example.com/myapp/?page=3的请求中，URLconf 也会查找 myapp / 。

### path() argument: view

当 Django 找到一个匹配模式时，它调用指定的视图函数，其中以 HttpRequest 对象作为第一个参数，并将 route 中的任何“捕获”值作为关键字参数。 我们稍后会给出一个例子。

### path() argument: kwargs

可以在字典中将任意关键字参数传递给目标视图。 我们不打算在本教程中使用 Django 的这个特性。

### path() argument: name

命名 URL 可以让您明确地从 Django 中的其他地方引用它，尤其是从模板中。 这个功能强大的特性允许您对项目的 URL 模式进行全局更改，同时只触及单个文件。

当您熟悉了基本的请求和响应流后，请阅读本教程的第2部分以开始使用数据库。




