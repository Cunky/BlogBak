---
title: Typecho的安装及使用
date: 2018-08-25 17:35:14
urlname: typecho-install
categories:
- 笔记
tags:
- Typecho
- Web
---
## 关于Typecho ##
Typecho是一个简单而强大的PHP博客平台
> Typecho is a PHP Blogging Platform. Simple and Powerful.
官网：[https://typecho.org][1]
官方介绍:[http://typecho.org/about][2]
## 各种平台 ##
LNMP环境：[https://lnmp.org][3]   #我将使用lnmp的一键安装脚本来快捷安装
Typecho文档：[http://docs.typecho.org][4]
Typecho常见问题：[http://docs.typecho.org/faq][5]
## 安装 ##
要安装该博客平台 我们需要LNMP环境
首先我们需要获取LNMP的安装文件

    apt-get -y install screen
    screen -S lnmp      #由于安装耗时较长 建议使用screen来防止网络突然断开
    wget http://soft.vpser.net/lnmp/lnmp1.5-full.tar.gz

> 文件大小：715MB MD5：5fb265d00abab3a6022f8b9a0c263ff4

具体国内外源可以参考[lnmp.org][6]提供的下载说明：[https://lnmp.org/download.html][7]
安装LNMP

    #解压并安装LNMP
     tar zxf lnmp1.5-full.tar.gz && cd lnmp1.5-full && ./install.sh lnmp

安装配置方案如下

    +------------------------------------------------------------------------+
    |          LNMP V1.5 for Ubuntu Linux Server, Written by Licess          |
    +------------------------------------------------------------------------+
    |        A tool to auto-compile & install LNMP/LNMPA/LAMP on Linux       |
    +------------------------------------------------------------------------+
    |           For more information please visit https://lnmp.org           |
    +------------------------------------------------------------------------+
    You have 10 options for your dateBase install.
    1: Install MySQL 5.1.73
    2: Install MySQL 5.5.60 (Default)
    3: Install MySQL 5.6.40
    4: Install MySQL 5.7.22
    5: Install MySQL 8.0.11
    6: Install MariaDB 5.5.60
    7: Install MariaDB 10.0.35
    8: Install MariaDB 10.1.33
    9: Install MariaDB 10.2.14
    0: DO NOT Install MySQL/MariaDB
    Enter your choice (1, 2, 3, 4, 5, 6, 7, 8, 9 or 0):       #可以直接回车使用默认方案
    No input,You will install MySQL 5.5.60
    ===========================
    Please setup root password of MySQL.
    Please enter: cunky812819
    MySQL root password: [password]      #输入MySQL的密码
    ===========================
    Do you want to enable or disable the InnoDB Storage Engine?
    Default enable,Enter your choice [Y/n]: y      #可以选择y也可以n
    You will enable the InnoDB Storage Engine
    ===========================
    You have 8 options for your PHP install.
    1: Install PHP 5.2.17
    2: Install PHP 5.3.29
    3: Install PHP 5.4.45
    4: Install PHP 5.5.38
    5: Install PHP 5.6.36 (Default)
    6: Install PHP 7.0.30
    7: Install PHP 7.1.18
    8: Install PHP 7.2.6
    Enter your choice (1, 2, 3, 4, 5, 6, 7 or 8):       #可以直接回车使用默认方案
    No input,You will install PHP 5.6.36
    ===========================
    You have 3 options for your Memory Allocator install.
    1: Don\'t install Memory Allocator. (Default)
    2: Install Jemalloc
    3: Install TCMalloc
    Enter your choice (1, 2 or 3):       #可以直接回车使用默认方案
    No input,You will not install Memory Allocator.
    
    Press any key to install...or Press Ctrl+c to cancel      #按任意按键继续或按Ctrl+C终止安装
------------------10 Years Later----------------------
十年后...我们终于把LNMP环境装完了 接下来可以使用`lnmp help`来获取帮助
注：接下来所有网站名字都直接按www.example.com和example.com来表示 请将其直接替换成自己的域名
我们直接新建一个vhost

    lnmp vhost add      #添加vhost 接下来的配置基本默认即可 如下
    +-------------------------------------------+
    |    Manager for LNMP, Written by Licess    |
    +-------------------------------------------+
    |              https://lnmp.org             |
    +-------------------------------------------+
    Please enter domain(example: www.lnmp.org): www.example.com      #输入域名
     Your domain: www.example.com
    Enter more domain name(example: lnmp.org *.lnmp.org): example.com      #输入更多域名
     domain list: example.com
    Please enter the directory for the domain: www.example.com
    Default directory: /home/wwwroot/www.example.com:       #输入网站根目录文件夹
    Virtual Host Directory: /home/wwwroot/www.example.com
    Allow Rewrite rule? (y/n) y      #启用重写规则
    Please enter the rewrite of programme, 
    wordpress,discuzx,typecho,thinkphp,laravel,codeigniter,yii2 rewrite was exist.
    (Default rewrite: other): typecho      #输入typecho
    You choose rewrite: typecho
    Enable PHP Pathinfo? (y/n) y      #启用PHP Pathinfo
    Enable pathinfo.
    Allow access log? (y/n) y      #启用日志文件
    Enter access log filename(Default:www.example.com.log): 
    You access log filename: www.example.com.log      #日志文件存储位置
    Create datebase and MySQL user with same name (y/n) y      #是否创建一个新的数据库 创建可以方便管理
    Enter current root password of datebase (Password will not shown):       #输入MySQL数据库管理密码
    OK, MySQL root password correct.
    Enter datebase name: typecho_db      #输入新数据库名
    Your will create a datebase and MySQL user with same name: typecho_db
    Please enter password for mysql user typecho_db: [password]      #输入新数据库密码
    Your password: [password] 
    Add SSL Certificate (y/n) n      #是否添加SSL证书
    
    Press any key to start create virtul host...      #按任意按键继续或按Ctrl+C终止安装
创建完成后我们开始为网站配置SSL证书
一般证书都会有两个文件：cert.pem和privkey.pem 或者 1_www.example.com_bundle.crt与2_www.example.com.key
这里我们使用的是Let\'s encrypt的免费SSL证书
具体获取过程请参考 [如何申请Let\'s encrypt的免费SSL证书][8] 这篇笔记
接着 我们进入网站的管理文件`/usr/local/nginx/conf/vhost/www.example.com.conf`
找到如下行
    listen 80;
    #listen [::]:80;
    server_name www.example.com example.com;
将其修改成

    listen 443 ssl;
    server_name example.com www.example.com;
    ssl_certificate /etc/letsencrypt/live/example.com/cert.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
并在最后结尾处添加

    server 
        {
            listen 80;
            listen [::]:80;
            server_name example.com www.example.com;
            return 301 https://$server_name$request_uri;
        }
最终文件应该是如下配置

    server
        {
            listen 443 ssl;
            server_name example.com www.example.com;
            ssl_certificate /etc/letsencrypt/live/example.com/cert.pem;
            ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
            index index.html index.htm index.php default.html default.htm default.php;
            root  /home/wwwroot/www.example.com;
    
            include rewrite/typecho.conf;
            #error_page   404   /404.html;
    
            # Deny access to PHP files in specific directory
            #location ~ /(wp-content|uploads|wp-includes|images)/.*\\.php$ { deny all; }
    
            include enable-php-pathinfo.conf;
    
            location ~ .*\\.(gif|jpg|jpeg|png|bmp|swf)$
            {
                expires      30d;
            }
    
            location ~ .*\\.(js|css)?$
            {
                expires      12h;
            }
    
            location ~ /.well-known {
                allow all;
            }
    
            location ~ /\\.
            {
                deny all;
            }
    
            access_log  /home/wwwlogs/www.example.com.log;
        }
    server {
            listen 80;
            listen [::]:80;
            server_name cunoe.com www.example.com;
            return 301 https://$server_name$request_uri;
           }
至此我们已经将SSL证书配置完毕
接下来我们开始正式安装Typecho

    cd /home/wwwroot/www.example.com      #进入网站根目录
    #下载并解压typecho
    wget http://typecho.org/downloads/1.1-17.10.30-release.tar.gz && tar -xvf 1.1-17.10.30-release.tar.gz && cd build && mv ./* .. && cd .. && rm -rf ./build && rm -f 1.1-17.10.30-release.tar.gz
接下来你就可以直接在浏览器上输入`https://www.example.com/install.php`来进行typecho的安装
![Typecho安装界面][9]

完成安装后就可以使用Typecho博客平台啦~
具体使用方法可以参考官方的文档：[http://docs.typecho.org][10]

笔记最后更新时间 `2018-08-25 17:35`


  [1]: https://typecho.org
  [2]: http://typecho.org/about
  [3]: https://lnmp.org
  [4]: http://docs.typecho.org
  [5]: http://docs.typecho.org/faq
  [6]: https://lnmp.org
  [7]: https://lnmp.org/download.html
  [8]: https://www.cunoe.com/index.php/notes/ssl-free.html
  [9]: https://www.cunoe.com/usr/uploads/typecho-install-1.PNG
  [10]: http://docs.typecho.org