---
title: Django及uWSGI的部署方案
date: 2019-1-31 23:45:44
urlname: django
categories:
- 笔记
tags:
- Django
- Python
---
## 开始 ##
这篇文章为我在2019年1月31日部署Django的笔记，具体环境如下：

    System：Ubuntu 16.04.5 LTS
    Web Server:Nginx/1.10.3 uWSGI/2.0.17.1
    Other:Django/2.1.5 Python/3.7[Anaconda]
    注：Django是一个高层次的Python Web框架
    Nginx是一个免费开源且高性能的HTTP服务器和反向代理
目前使用Nginx和uWSGI部署Django项目还是比较主流的方案之一，也是一种可靠而简单的方案
具体组件栈如下所示

> the web client <-> Nginx <-> the socket <-> uWSGI <-> Django
## 设置uWSGI前 ##
> 以下全文域名均以 `example.com` 为例 端口将以 `8000` 为例
### Django ###
将Django安装至你的服务器上并创建[或将本地项目复制到服务器上]

    pip install Django==2.1.5
    django-admin.py startproject myWeb    # 新建Django项目
    cd myWeb
## uWSGI安装以及配置 ##
### uWSGI安装 ###

    pip install uwsgi
这不是唯一的安装uWSGI的方式，但是我认为比较好的方案
需要注意的是，安装uWSGI之前要记得在服务器上安装Python的开发包，以免出现各种错误
我的系统为`apt-get install python3.7-dev`
但我安装的过程中还出现了不一样的错误 具体log[\[Here\]][1]
经过一番排查 发现是gcc的版本问题 服务器自带的gcc版本为5.4的 需要降级到4.7

    sudo apt-get install gcc-4.7
    sudo rm /usr/bin/gcc
    sudo ln -s /usr/bin/gcc-4.7 /usr/bin/gcc
### 简单测试 ###
创建一个名为`test.py`的文件：

    def application(env, start_response):
        start_response('200 OK', [('Content-Type','text/html')])
        return [b"Hello World"] # python3
        # return ["Hello World"] # python2
> 要注意Python 3和Python 2的区别
运行uWSGI：

    uwsgi --http :8000 --wsgi-file test.py
 - http :8000：指使用http协议，端口8000
 - wsgi-file：加载指定文件

此时可以通过浏览器访问8000端口:

> http://example.com:8000
并得到一个`Hello World`的返回值
### 加载Django项目 ###
基础测试正常后，我们可以开始考虑部署到Django项目上
首先，我们需要确保Django项目的正常工作

    python manage.py runserver
而后可以通过如下指令将uWSGI接入Django

    uwsgi --http :8000 --module myWeb.wsgi
 - module：加载指定wsgi模块
此时你可以通过浏览器访问

> http://example.com:8000
## Nginx ##
### 安装Nginx ###

    sudo apt-get install nginx
    sudo /etc/init.d/nginx start
此时，你可以通过浏览器访问80端口来查看Nginx是否正常工作
一般来说 你会得到一个Welcome to nginx的信息
## 为Django配置nginx ##
你需要一个`uwsgi_params`文件，可以在[https://github.com/nginx/nginx/blob/master/conf/uwsgi_params][2]这里找到。
请将其复制下来并粘贴到你的项目目录中

接着创建一个`myWeb_nginx.conf`文件并写入如下信息

    # myWeb_nginx.conf
    
    # the upstream component nginx needs to connect to
    upstream django {
        server unix:///path/myWeb/myWeb.sock; # for a file socket
        # server 127.0.0.1:8001; # for a web port socket (we'll use this first)
    }
    
    # configuration of the server
    server {
        # the port your site will be served on
        listen      8000;
        # the domain name it will serve for
        server_name .example.com; # substitute your machine's IP address or FQDN
        charset     utf-8;
    
        # max upload size
        client_max_body_size 75M;   # adjust to taste
    
        # Django media
        location /media  {
            alias /path/myWeb/media;  # your Django project's media files - amend as required
        }
    
        location /static {
            alias /path/myWeb/static; # your Django project's static files - amend as required
        }
    
        # Finally, send all non-media requests to the Django server.
        location / {
            uwsgi_pass  django;
            include     /path/myWeb/uwsgi_params; # the uwsgi_params file you installed
        }
    }
> 注：这里我们使用了Unix socket

并自行修改里面的具体信息
最后将文件链接到sites-enabled

    sudo ln -s /path/myWeb/myWeb_nginx.conf /etc/nginx/sites-enabled
### Django静态文件部署 ###
在Django项目中 编辑myWeb/settings.py
在最后面添加`STATIC_ROOT = os.path.join(BASE_DIR, "static/")`
接着运行

    python manage.py collectstatic
## Nginx和uWSGI ##
输入`uwsgi --socket myWeb.sock --wsgi-file test.py`并在浏览器中访问以测试nginx是否正常运行

如果出现无法访问，可以尝试如下命令

    uwsgi --socket myWeb.sock --wsgi-file test.py --chmod-socket=666
给nginx权限去使用这个socket
测试通过后可以使用如下指令来运行Django应用

    uwsgi --socket myWeb.sock --module myWeb.wsgi --chmod-socket=666
测试通过后我们在项目目录创建一个`myWeb_uwsgi.ini`文件来方便我们启动

    # myWeb_uwsgi.ini file
    [uwsgi]
    
    # Django-related settings
    # the base directory
    chdir           = /path/project
    # Django's wsgi file
    module          = project.wsgi
    
    # process-related settings
    # master
    master          = true
    # maximum number of worker processes
    processes       = 10
    # the socket (use the full path to be safe
    socket          = /path/project/myWeb.sock
    # ... with appropriate permissions - may be needed
    chmod-socket    = 666
    # clear environment on exit
    vacuum          = true
最后使用这个文件运行uWSGI并测试Django站点是否正常工作

    uwsgi --ini myWeb_uwsgi.ini
  [1]: https://www.cunoe.com/usr/uploads/uwsgi-log.txt
  [2]: https://github.com/nginx/nginx/blob/master/conf/uwsgi_params
