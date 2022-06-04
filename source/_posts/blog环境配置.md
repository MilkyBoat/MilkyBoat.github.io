---
title: blog环境配置
date: 2022-06-04 10:42:01
tags: 
    - blog
    - webhook 
    - hexo
---

# 完美代替gitpage的个人博客方案：
## hexo + 第三方git托管 + 自建web服务器 + webhook自动部署

基于hexo建立的博客站点，编写者需要在本地写好markdown格式的blog之后，由框架处理生成静态页面，随后通过git将静态页面发布到目标git服务器。如果git服务器选用github的话，可以免费使用其gitpage服务作为web服务器。但是githu服务器在外网不够稳定，于是最佳方案变成了使用自建web服务器+通过webhook自动同步部署（或者简单一点定时clone）。

---

## 本地：hexo

先装一个[Nodejs](https://nodejs.org/zh-cn/download/)。安装完成后，如果命令行输入`node -v`能显示`nodejs`版本号，说明安装成功。

然后安装[git](https://nodejs.org/zh-cn/download/)。同理，安装完成后，如果命令行输入`git --version`能显示`git`版本号，说明安装成功。

然后给npm换个源，这个东西随时都在变，现场百度找最新的教程最管用。

接下来是正式安装

```bash
npm install hexo-cli -g
```

完成后，如果`hexo -v`能输出一堆版本号就说明安装好了。

接下来hexo会自动产生一个hello world博客页面，上面说明了如何使用，就像这篇[hello-world](/2019/11/02/hello-world/)

这里介绍了一些基本的使用，比如使用`hexo new "文章名"`新建博文，使用`hexo g`渲染出静态页面，使用`hexo s`在本地`localhost:4000`预览web效果，这个命令可以用`hexo s -p 80`指定端口号为80，使用`hexo d`发布到git服务器（需要配置，详见后文[git服务器配置](#git服务器配置)。

如果你想了解更多，可以查阅[官方文档](https://hexo.io/docs/)

---

## 服务器：web server + webhook (宝塔工具)

服务器方面，博客需要一个git服务器来存放博客文件，一个web服务器对外提供网页浏览服务，它们二者如果不是部署在同一台服务器上则需要一个webhook来实现自动同步。

这里git服务器选用github，web服务器自建，设置webhook使得每次github仓库收到新提交时，web服务器都能立刻自动同步。

### git服务器配置

github/gitee均可，直接开个新仓库就行了，github的话还可以开gitpage直接让github充当博客的web服务器，但是最近github.io进了北越南的黑名单访问困难，推荐自建web服务器。

当然git服务器也可以自建，但是毕竟自建服务器可能存在各种各样的不稳定因素，导致数据丢失等等极其严重的问题，推荐使用第三方git服务。

总之你现在在git服务器上建立好了一个空白仓库。

接下来在你的hexo项目根目录中找到`_config.yaml`，在末尾找到`Deployment`配置项，填入你的repo地址即可。

### web服务器配置

如果不想打理，直接装一个[宝塔](https://www.bt.cn/new/download.html)。用nginx作为静态网站服务器。新建一个纯静态网站即可，建好之后复制一下网站部署根目录备用。

### webhook配置

这里使用宝塔Linux面板的WebHook工具来实现自动部署。

在面板的软件商店搜索webhook，安装后在工具界面点击`添加`。

![宝塔面板](bt_webhook.png)

WebHook名称随便写，脚本框粘贴以下代码【注意修改里面的两个地址】


```bash
#!/bin/bash
 
echo ""
#输出当前时间
date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"
echo "Start"
#判断宝塔WebHook参数是否存在
if [ ! -n "$1" ];
then 
    echo "param参数错误"
    echo "End"
    exit
fi
#git项目路径
gitPath="/data/wwwroot/blog.milkyship.cn"  # 【这里填入网站的部署路径】
#git 网址
gitHttp="https://github.com/MilkyBoat/$1.git"  # 【这里填入你的git url】
echo "Web站点路径：$gitPath"

#判断项目路径是否存在
if [ ! -d "$gitPath" ]; then
    echo "该项目路径不存在"
    echo "新建项目目录"
    mkdir $gitPath
    cd $gitPath
fi

#判断是否存在git目录
if [ ! -d ".git" ]; then
    echo "在该目录下克隆 git"
    sudo git clone $gitHttp gittemp
    sudo mv gittemp/.git .
    sudo rm -rf gittemp
fi

echo "拉取最新的项目文件"
#sudo git reset --hard origin/master
sudo git pull        
echo "设置目录权限"
sudo chown -R www:www $gitPath
echo "End"
exit

```

这一代码将会在WebHook触发的时候检查有无对应目录，并且拉取对应仓库，具体使用哪个仓库你可以通过在WebHook发送端设置参数来完成。

接下来，在刚刚添加的WebHook里点击`查看密钥`

![密钥界面](res_webhook.png)

这里面密钥用于保证你的WebHook不会被错误的触发或者盗用，url的get请求参数可以被webhook识别，在脚本中用`$1`访问。

然后前往github配置webhook。

![github webhook配置](github_webhook.png)

这里的`Payload Url`填入刚刚的url，`Secret`填入密钥，尤其要注意`Content type`要选择json，否则可能被nginx拦截（根据你宝塔web服务器的配置）。填好之后点击`Add webhook`即可。接下来github回立刻发送一次请求进行测试，如果你在webhooks列表里面看到一个绿色√，那么webhook就已经配置好了，点`edit` - `Recent Deliveries`可以看到近期webhook触发结果，正常情况应当是http200，返回的http response body为`{"code": 1}`。

![正确配置的webhook](suc_webhook.png)

### 

至此，一个能够自动同步部署、完美代替gitpage的个人博客站点建立完成。
