---
title: 免费SSL申请及使用
date: 2018-08-25 19:11
urlname: free-ssl
categories: 
- 笔记
tags:
- SSL
---
## Let's  encrypt ##
[Let's Encrypt][1]作为一个公共且免费SSL的项目逐渐被广大用户传播和使用，是由Mozilla、Cisco、Akamai、IdenTrust、EFF等组织人员发起，主要的目的也是为了推进网站从HTTP向HTTPS过度的进程，目前已经有越来越多的商家加入和赞助支持。
Let's encrypt官网：[https://letsencrypt.org/][2]

## 安装客户端 ##
我们需要安装一个Let\'s encrypt的客户端

    #安装客户端
    sudo apt-get install letsencrypt

Let's encrypt客户端的名称可以是`letsencrypt`或者`certbot` 请使用`which`指令查看你是哪一种 后文统一采用`letsencrypt`指令

接下来我们来获取证书
注：接下来所有网站名字都直接按`www.example.com`和`example.com`来表示 请将其直接替换成自己的域名
由于客户端需要使用80和443端口 先将占用该端口的程序关闭 通常关闭Nginx即可
接下来输入以下指令来获取证书

    sudo letsencrypt certonly --standalone
配置方案如下


    Saving debug log to /var/log/letsencrypt/letsencrypt.log
    Plugins selected: Authenticator standalone, Installer None
    Enter email address (used for urgent renewal and security notices) (Enter \'c\' to cancel): example@example.com      #输入邮箱
    -------------------------------------------------------------------------------
    Please read the Terms of Service at https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
    gree in order to register with the ACME server at https://acme-v01.api.letsencrypt.org/directory
    -------------------------------------------------------------------------------
    (A)gree/(C)ancel: a      #同意服务许可
       
    Please enter in your domain name(s) (comma and/or space separated)  (Enter \'c\'
    to cancel): example.com www.example.com      #输入服务器域名
    Obtaining a new certificate      #坐等客户端进行身份验证 证书与相关信息会储存在`/etc/letsencrypt`下
    Performing the following challenges:
    http-01 challenge for example.com
    http-01 challenge for www.example.com
    Cleaning up challenges
    IMPORTANT NOTES:
     - Congratulations! Your certificate and chain have been saved at:
       /etc/letsencrypt/live/example.com/fullchain.pem
       Your key file has been saved at:
       /etc/letsencrypt/live/example.com/privkey.pem
       Your cert will expire on 2018-11-23. To obtain a new or tweaked
       version of this certificate in the future, simply run certbot
       again. To non-interactively renew *all* of your certificates, run
       \"certbot renew\"
     - If you like Certbot, please consider supporting our work by:
       Donating to ISRG / Let\'s Encrypt:   https://letsencrypt.org/donate
       Donating to EFF:                    https://eff.org/donate-le
至此 Let's encrypt免费SSL证书已经申请完毕
最后进行SSL证书的部署
首先 我们进入网站的管理文件`/usr/local/nginx/conf/vhost/www.example.com.conf`
找到如下行

    listen 80;
    #listen [::]:80;
    server_name www.example.com example.com;
将其修改成

    listen 443 ssl;
    server_name example.com www.example.com;
    ssl_certificate /etc/letsencrypt/live/example.com/cert.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
最后在结尾处添加

    server 
        {
            listen 80;
            listen [::]:80;
            server_name example.com www.example.com;
            return 301 https://$server_name$request_uri;
        }
最终配置文件如下

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
            server_name example.com www.example.com;
            return 301 https://$server_name$request_uri;
           }

重启Nginx服务即可完成部署

笔记最后更新时间 `2018-08-25 19:11`

  [1]: https://letsencrypt.org/
  [2]: https://letsencrypt.org/
