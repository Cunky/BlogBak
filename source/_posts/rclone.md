---
title: Rclone的安装及挂载使用
date: 2018-08-02 16:46:44
urlname: rclone
categories:
- 笔记
tags:
- VPS
- 工具
---
## Rclone是啥玩意儿？ ##
Rclone是一款命令行工具，支持在不同对象储存、网盘之间上传、下载以及同步数据
Rclone官网：[https://rclone.org][1]
GitHub项目：[https://github.com/ncw/rclone][2]
支持多种主流对象储存 如下表

    Amazon Drive   ([See note][3])
    Amazon S3  
    Backblaze B2  
    Box  
    Ceph  
    DigitalOcean Spaces  
    Dreamhost  
    Dropbox  
    FTP  
    Google Cloud Storage  
    Google Drive  
    HTTP  
    Hubic  
    IBM COS S3  
    Memset Memstore  
    Mega  
    Microsoft Azure Blob Storage  
    Microsoft OneDrive  
    Minio  
    Nextcloud  
    OVH  
    OpenDrive  
    Openstack Swift  
    Oracle Cloud Storage  
    ownCloud  
    pCloud  
    put.io  
    QingStor  
    Rackspace Cloud Files  
    SFTP  
    Wasabi  
    WebDAV  
    Yandex Disk  
    The local filesystem
  
这里我们以Google Drive为例

## 安装 ##

> 系统简介
> Ubuntu 18.04
> Google Drive

在[Rclone官网][4]下载其安装文件
同时安装Rclone

    #下载并安装Rclone
    wget https://downloads.rclone.org/v1.42/rclone-v1.42-linux-amd64.deb
    dpkg -i rclone-v1.42-linux-amd64.deb

很简单的就将其安装完毕 接下来我们开始配置Rclone

    #配置Rclone
    rclone config
配置方案如下 手动操作的位置会加上注释

    No remotes found - make a new one
    n) New remote
    s) Set configuration password
    q) Quit config
    n/s/q> n      #这里输入n表示新建一个配置
    name> gd      #输入自定义的配置名
    Type of storage to configure.
    Choose a number from below, or type in your own value
     1 / Alias for a existing remote
       \\ \"alias\"
     2 / Amazon Drive
       \\ \"amazon cloud drive\"
     3 / Amazon S3 Compliant Storage Providers (AWS, Ceph, Dreamhost, IBM COS, Minio)
       \\ \"s3\"
     4 / Backblaze B2
       \\ \"b2\"
     5 / Box
       \\ \"box\"
     6 / Cache a remote
       \\ \"cache\"
     7 / Dropbox
       \\ \"dropbox\"
     8 / Encrypt/Decrypt a remote
       \\ \"crypt\"
     9 / FTP Connection
       \\ \"ftp\"
    10 / Google Cloud Storage (this is not Google Drive)
       \\ \"google cloud storage\"
    11 / Google Drive
       \\ \"drive\"
    12 / Hubic
       \\ \"hubic\"
    13 / Local Disk
       \\ \"local\"
    14 / Mega
       \\ \"mega\"
    15 / Microsoft Azure Blob Storage
       \\ \"azureblob\"
    16 / Microsoft OneDrive
       \\ \"onedrive\"
    17 / OpenDrive
       \\ \"opendrive\"
    18 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
       \\ \"swift\"
    19 / Pcloud
       \\ \"pcloud\"
    20 / QingCloud Object Storage
       \\ \"qingstor\"
    21 / SSH/SFTP Connection
       \\ \"sftp\"
    22 / Webdav
       \\ \"webdav\"
    23 / Yandex Disk
       \\ \"yandex\"
    24 / http Connection
       \\ \"http\"
    Storage> 11      #注意：这里的Google Cloud Storage不是Google Drive
    Google Application Client Id - leave blank normally.
    client_id>       #为了采用自动配置 接下来几个选项直接回车
    Google Application Client Secret - leave blank normally.
    client_secret>       #回车
    Scope that rclone should use when requesting access from drive.
    Choose a number from below, or type in your own value
     1 / Full access all files, excluding Application Data Folder.
       \\ \"drive\"
     2 / Read-only access to file metadata and file contents.
       \\ \"drive.readonly\"
       / Access to files created by rclone only.
     3 | These are visible in the drive website.
       | File authorization is revoked when the user deauthorizes the app.
       \\ \"drive.file\"
       / Allows read and write access to the Application Data folder.
     4 | This is not visible in the drive website.
       \\ \"drive.appfolder\"
       / Allows read-only access to file metadata but
     5 | does not allow any access to read or download file content.
       \\ \"drive.metadata.readonly\"
    scope>       #回车 
    ID of the root folder - leave blank normally.  Fill in to access \"Computers\" folders. (see docs).
    root_folder_id>       #回车 
    Service Account Credentials JSON file path  - leave blank normally.
    Needed only if you want use SA instead of interactive login.
    service_account_file>       #回车 
    Remote config
    Use auto config?
     * Say Y if not sure
     * Say N if you are working on a remote or headless machine or Y didn\'t work
    y) Yes
    n) No
    y/n> n      #输入n表示采用自动配置
    If your browser doesn\'t open automatically go to the following link: [Link]      #这里会出现一长串的网址 将其复制到浏览器上并访问 登陆后即可获得一串[verification code]
    Log in and authorize rclone for access
    Enter verification code> [verification code]      #这里输入你获取的[verification code]
    Configure this as a team drive?
    y) Yes
    n) No
    y/n> n      #选择是否为团队网盘
    --------------------
    [gd]
    type = drive
    client_id = 
    client_secret = 
    scope = 
    root_folder_id = 
    service_account_file = 
    token = {[token]}
    --------------------
    y) Yes this is OK
    e) Edit this remote
    d) Delete this remote
    y/e/d> y      #输入y表示确认该配置
    Current remotes:
    
    Name                 Type
    ====                 ====
    gd                   drive
    
    e) Edit existing remote
    n) New remote
    d) Delete remote
    r) Rename remote
    c) Copy remote
    s) Set configuration password
    q) Quit config
    e/n/d/r/c/s/q> q      #输入q退出配置过程
至此已经成功配置完成Rclone

## 使用 ##
Rclone 命令的语法格式

    Syntax: [options] subcommand <parameters> <parameters...>
常用命令如下表

    rclone config - 以控制会话的形式添加rclone的配置，配置保存在.rclone.conf文件中。
    rclone copy - 将文件从源复制到目的地址，跳过已复制完成的。
    rclone sync - 将源数据同步到目的地址，只更新目的地址的数据。
    rclone move - 将源数据移动到目的地址。
    rclone delete - 删除指定路径下的文件内容。
    rclone purge - 清空指定路径下所有文件数据。
    rclone mkdir - 创建一个新目录。
    rclone rmdir - 删除空目录。
    rclone check - 检查源和目的地址数据是否匹配。
    rclone ls - 列出指定路径下所有的文件以及文件大小和路径。
    rclone lsd - 列出指定路径下所有的目录/容器/桶。
    rclone lsl - 列出指定路径下所有文件以及修改时间、文件大小和路径。
    rclone md5sum - 为指定路径下的所有文件产生一个md5sum文件。
    rclone sha1sum - 为指定路径下的所有文件产生一个sha1sum文件。
    rclone size - 获取指定路径下，文件内容的总大小。.
    rclone version - 查看当前版本。
    rclone cleanup - 清空remote。
    rclone dedupe - 交互式查找重复文件，进行删除/重命名操作。

## 挂载到本地 ##
这一步很简单
首先我们创建一个挂载点

    #创建挂载点
    mkdir [path]      #[path]表示挂载点路径 可自定义
接着将Drive挂载到[path]

    #挂载Drive
    rclone mount [name]:/ [path] --allow-other --allow-non-empty --vfs-cache-mode writes
    #[name]表示Drive的配置名 [path]表示挂载点路径 可自定义 但必须存在该路径
输入完这条指令后就没什么动静了 等几分钟后我们手动断开和服务器的连接 再重新用SSH登录就可以了
至此 我们已经成功挂载了Google Drive

    #查看是否成功挂载
    df -Th
看到返回的值里面出现了

    Filesystem     Type         Size  Used Avail Use% Mounted on
    udev           devtmpfs     463M     0  463M   0% /dev
    tmpfs          tmpfs         99M  652K   98M   1% /run
    /dev/vda1      ext4          25G  5.2G   19G  22% /
    tmpfs          tmpfs        493M     0  493M   0% /dev/shm
    tmpfs          tmpfs        5.0M     0  5.0M   0% /run/lock
    tmpfs          tmpfs        493M     0  493M   0% /sys/fs/cgroup
    tmpfs          tmpfs         99M     0   99M   0% /run/user/0
    [name]:        fuse.rclone   50G   10G   50G  20% [path]      #Rclone挂载点
笔记最后更新时间 `2018-08-02 16:46`
  [1]: https://rclone.org
  [2]: https://github.com/ncw/rclone
  [3]: https://rclone.org/amazonclouddrive/#status
  [4]: https://rclone.org