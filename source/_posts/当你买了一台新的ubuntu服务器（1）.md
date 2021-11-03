---
title: 当你买了台新的ubuntu服务器(1)
date: 2020-05-07 10:39:02
tags: 
    - 服务器 
    - 运维
    - 服务器配置
---

# 当你买了台新服务器-ubuntu服务器常见设置


## 创建新用户!
操作服务器绝对不要天天用root，迟早会出事，所以新服务器第一件事情就是来个普通权限的用户。

（以下指令均在root权限下运行）
``` bash
$ adduser ubuntu
```
其中ubuntu可以替换为你想要的用户名，之后输入密码（不会显示在屏幕上，盲输就行），然后输入用户信息就可以了。然后分配sudo权限：
```bash
$ visudo
```
这里没有漏空格，就是visudo，进去之后是专门的编辑器，编辑模式默认是插入，找到“root ALL=(ALL:ALL) ALL”这一行，在下面加上这一行，同样的，ubuntu替换为你的用户名
```
ubuntu ALL=(ALL:ALL) ALL
```
这个奇奇怪怪的编辑器使用ctrl+o（然后再按enter）保存，ctrl+c取消，ctrl+x退出。然后使用
```bash
$ su ubuntu
```
切换到刚刚创建的用户。

此外创建用户还有“useradd”命令，但是用这个创建命令似乎不能单独登录，不要用就是了。

----
## 延长ssh自动断开时间
如果通过终端ssh连接服务器时，当鼠标和键盘长时间不操作，服务器就会自动断开连接。
* 方法一、
修改/etc/ssh/sshd_config配置文件，找到ClientAliveCountMax（单位为分钟）修改为所需要的值，然后执行
```bash
$ service sshd reload
```
* 方法二、
找到所在用户的.ssh目录,如root用户该目录在：/root/.ssh/,在该目录创建config文件
```bash
$ vi /root/.ssh/config
```
加入下面一句：ServerAliveInterval 60 保存退出，再ssh服务器的时候，不会因为长时间操作断开。加入这句之后，ssh客户端会每隔一段时间自动与ssh服务器通信一次，所以长时间操作不会断开。
* 方法三、
修改/etc/profile配置文件
```bash
$ vi /etc/profile
```
增加：TMOUT=1800 这样30分钟无操作就自动LOGOUT


----
## 换源（仅中国大陆地区服务器需要）
这里一次性汇总一下所有常见的工具的换源

#### apt-get
先备份原始文件
```
$ sudo cp /etc/apt/sources.list /etc/apt/sources.bak1
#第一个参数时拷贝的文件路径和文件名称,第二个是拷贝到(粘贴)的文件路径和文件名
```
然后开始编辑源地址
```bash
$ sudo vi /etc/apt/sources.list
```
删掉或注释掉原有的国外地址。然后去如下地址任意一个获取最新的镜像站点地址，复制到 /etc/apt/sources.list 中即可。

* [阿里云开源镜像站](https://developer.aliyun.com/mirror/ubuntu)

* [清华大学开源镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

* [网易开源镜像站](http://mirrors.163.com/.help/ubuntu.html)

换源之后进行软件包更新（可选）
```bash
$ sudo apt-get update
# 这个命令，会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑。
$ sudo apt-get upgrade
# 这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新。如果你的软件都是最新版本，会提示：
# 升级了 0 个软件包，新安装了 0 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。
```
#### pip

新服务器最好检查一下python版本，推荐使用3.7，如果python版本过低，使用如下步骤升级。我的服务器是 ubuntu 16 预装 python 3.5，这里升级到python 3.7
```bash
$ sudo apt-get install zlib1g-dev libbz2-dev libssl-dev libncurses5-dev libsqlite3-dev libreadline-dev tk-dev libgdbm-dev libdb-dev libpcap-dev xz-utils libexpat1-dev liblzma-dev libffi-dev libc6-dev
# 安装依赖包

# $ mkdir package # 创建新的文件夹保存包（可选）
# $ cd package

$ wget 'https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz'
# 官网下载安装包
$ tar zxvf Python-3.7.3.tgz
# 解压
$ cd Python-3.7.3
$ sudo mkdir -p /usr/local/python3
# 创建安装目标文件夹
# ！！！注意！！！这里使用 usr 目录会导致 pip 安装一些包的时候需要管理员权限，如果不需要多用户使用可以安装到 /home/ubuntu/python3 目录
$ ./configure --prefix=/usr/local/python3  --enable-optimizations
$ make
$ sudo make install
# 安装

## 删除软链接
# 先执行查看版本，如果有则证明软链接已存在，需要先删去以前的再重新建立
$ sudo rm /usr/bin/python3
$ sudo rm /usr/bin/pip3
# 注意，只能删除软连接，不要把老版本 python 目录删掉！！！切记！！！


##建立新的软连接
#添加python3的软链接
$ sudo ln -s /usr/local/python3/bin/python3.7 /usr/bin/python3
#添加 pip3 的软链接
$ sudo ln -s /usr/local/python3/bin/pip3.7 /usr/bin/pip3

# 检测版本
$ python3 -V
$ pip3 -V

```

新服务器默认没有pip，使用如下指令安装，已经安装了的可以跳过
```bash
$ sudo apt-get install python3-pip
```

确定安装好pip之后，找到如下文件
```
~/.pip/pip.conf
```

vi编辑添加（这里为阿里云镜像）
```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

----
## 安装java

2019年起，oracal将官方的jdk设为付费下载，所以推荐安装open-jdk，版本方面推荐java-8（1.8）。

```bash
$ sudo apt install default-jdk      # java 10
$ sudo apt install openjdk-8-jdk    # java 8
```

> 如果使用 open jdk ，以上指令任选其一即可直接完成，以下所有步骤仅原版jdk安装需要
如果一定要使用oracal的原版jdk的话，必须自己下载安装包，这里举例 jdk-8u221-linux-x64.tar.gz 

```bash
$ sudo mkdir /usr/lib/java
$ sudo tar -zxvf jdk-8u212-linux-x64.tar.gz -C /usr/lib/java
$ sudo gedit ~/.bashrc  # 修改环境变量
```

文件尾追加以下内容（open-jdk不需要）

```
# set oracle jdk environment
export JAVA_HOME=/usr/lib/java/jdk1.8.0_212 ## 这里的目录换成自己解压的jdk 目录
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
```

```bash
$ source ~/.bashrc
# 使环境变量生效（open-jdk不需要）
$ sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk1.8.0_212/bin/java 300
# 系统注册此jdk（300为优先级）
```

如果我们在服务器上安装了多个Java版本，我们可以使用update-alternatives系统更改默认版本：
```bash
$ sudo update-alternatives --config java
```
在随后的界面中输入序号并回车以切换默认jdk

----
## 安装php

```bash
$ sudo apt-get install php
```
