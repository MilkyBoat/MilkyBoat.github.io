---
title: 服务器一键配置ssr
date: 2020-05-08 10:22:45
tags: 
    - 服务器 
    - 服务器配置
---

# 服务器一键配置ssr


## 服务器配置

> 要求系统支持 wget 命令，如果有wget命令直接开始下一步，如果不支持需要使用以下命令安装
```bash
$ sudo yum -y install wget  # cent os
$ sudo apt-get install wget  # ubuntu
```

一键安装脚本
```bash
$ wget https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
$ chmod +x shadowsocksR.sh
$ ./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

在脚本中设置
* 端口（建议大于80小于150，方便安卓端填入）
* 密码
* 加密方式
* 协议插件
* 混淆插件（当协议为origin时，混淆必须为plain，建议不使用tls开头的混淆）
* 设备数、单线程限速和端口总限速（建议默认）

随后按 y 确认安装（也可能没有这一步直接开始安装）

最后脚本会显示设置成功的ssr账号信息，务必妥善保存！！！！！！


## 安装ssr客户端

* [Windows SSR客户端（需要.Net2或4框架）](https://github.com/shadowsocksr-backup/shadowsocksr-csharp/releases)
* [Mac SSR客户端](https://github.com/shadowsocksr-backup/ShadowsocksX-NG/releases)
* [Linux SSR配置脚本](https://github.com/the0demiurge/CharlesScripts/blob/master/charles/bin/ssr)
* [Android SSR客户端](https://github.com/shadowsocksr-backup/shadowsocksr-android/releases/download/3.4.0.8/shadowsocksr-release.apk)
* [所有客户端下载备用链接](https://www.mediafire.com/folder/sfqz8bmodqdx5/shadowsocks%E7%9B%B8%E5%85%B3%E5%AE%A2%E6%88%B7%E7%AB%AF)

客户端填写之前保存的ssr账户信息，本地ip选择127.0.0.1（默认值），本地端口选择1080（默认值）

### // 现在长城上就有一条专属于你的隧道了