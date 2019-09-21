---
title: CUNOE FQ节点记录
date: 2019-09-21 12:00:00
urlname: gfw
sage: true
categories:
- 笔记
tags:
- 总结
---
##Supported by CUNOE.com
为方便翻墙，于此记录部分翻墙方案
其中服务器节点每两周更新一次
我们仍然推荐各位自行搭建翻墙的服务器 保证稳定性
#### V2ray (CUNOE POWER) 稳定运行
新技术 更稳定
V2rayN客户端下载地址(源地址)：https://github.com/2dust/v2rayN/releases
CUNOE备用地址：https://cloud.cunoe.com/Download/v2rayN-Core.zip
解压到任意地址(全英文) 以管理员权限打开V2rayN.exe 右下角会出现一个v一样的图标 左键单击 打开设置菜单 在本网站复制一个以`vmess://`开头的地址 在服务器列表处右键 选择从`剪切板导入批量URL` 勾选其为活动服务器 右键右下角的v一样的图标 选择启动HTTP代理 接着选择HTTP代理模式为开启HTTP代理

#### Shadowsocks(及其变种)(R)
最普遍的翻墙方法 其使用方案如下
1. 下载相应客户端 一般为ShadowsocksR 地址：https://www.mediafire.com/folder/btkdbx7j9lr98/Shadowsocks_%E7%9B%B8%E5%85%B3%E5%AE%A2%E6%88%B7%E7%AB%AF
2. 首先根据你系统安装 .NET Framework v2.0或4.0的支持库 来选择客户端中的 ShadowsocksR-dotnet2.0.exe 或 ShadowsocksR-dotnet4.0.exe 并打开客户端。
3. 这时候，右下角托盘图标会出现一个纸飞机，然后双击纸飞机图标，弹出菜单 按照本文给出的地址进行设置
![](https://www.cunoe.com/usr/uploads/ssr-1.png)
第二种设置方法 复制以`ssr://`开头的链接后 右键纸飞机图标 选择`剪切板批量导入ssr://链接`
4. 接着在纸飞机上右键弹出菜单 选择服务器 并打开系统代理模式 ---- 全局模式或PAC模式
![](https://www.cunoe.com/usr/uploads/ssr.png)

##### 节点
| 服务器位置  | 美国西雅图  | 美国洛杉矶  | 法国巴黎  | 日本东京  | 俄罗斯莫斯科  |
| ------------ | ------------ | ------------ | ------------ | ------------ | ------------ | 
| IP  | 162.245.239.74  | 67.21.94.192  | 104.238.190.132  |  194.156.231.13 | 45.129.2.171  |  
| 端口  | 40902  | 34781  | 18565  | 56  | 56  |  
| 密码  | dongtaiwang.com  | dongtaiwang.com  | cunoe007  |  lncn.org 5r | lncn.org 3b  |  
| 加密  | aes-256-cfb  | aes-256-cfb  | aes-256-cfb  | rc4  | rc4  |  
| 协议  |  origin | origin  | auth_chain_a  | origin  |  origin |  
| 混淆  | plain  | plain  | plain  |plain   |  plain |

## 由于临近国庆 几乎全部节点失效 建议自行搭建
