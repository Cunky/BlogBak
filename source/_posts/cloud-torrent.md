---
title: Cloud-Torrent安装及使用
date: 2018-10-02 22:08:00
urlname: cloud-torrent
categories:
- 笔记
tags: 
- VPS
- 工具
---
> 很多人买了VPS以后 会出现资源闲置的情况
> 今天就给大家介绍一下Cloud-Torrent这款简单好用（还行吧XD）的BT下载工具
## 链接 ##
GitHub项目：[https://github.com/jpillora/cloud-torrent][1]
## 准备 ##
你需要一台能用的VPS 我这里用的是一台OVZ的小机子
## 安装 ##
安装Cloud-Torrent十分简单
在终端输入

    curl https://i.jpillora.com/cloud-torrent! | bash
即可
附：Cloud-Torrent最新版本：[https://github.com/jpillora/cloud-torrent/releases/latest][5]
成功安装以后输入 `cloud-torrent --help` 可以查看帮助
![Cloud-Torrent Help][2]
接下来是启动Cloud-Torrent

    cloud-torrent \
      -t 'Cunky' \
      -p '9090' \
      -a 'cunky:testest0744'
完成后即可通过 http://ip:port 进入Web管理界面
注：cunky为用户名 testest0744为密码
其他安装方法 如docker等请参考GitHub的[Cloud-Torrent项目][3]
这里建议安装SSL证书保证安全性

## 使用 ##
如图
![Cloud-Torrent Web][4]
可以直接将种子拖进去 也可以直接上传 或者填写磁力链接
我这的机子测试最高到过50MB/s
这个也可以结合脚本和Rclone来用 都是不错的

笔记最后更新时间 `2018-10-02 22:08`

  [1]: https://github.com/jpillora/cloud-torrent
  [2]: https://www.cunoe.com/usr/uploads/Cloud-Torrent-1.png
  [3]: https://github.com/jpillora/cloud-torrent
  [4]: https://www.cunoe.com/usr/uploads/Cloud-Torrent-2.png
  [5]: https://github.com/jpillora/cloud-torrent/releases/latest