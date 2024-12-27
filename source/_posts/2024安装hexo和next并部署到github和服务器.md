---
title: 2024安装hexo和next并部署到github和服务器
abbrlink: 950
tags:
  - 服务器
  - 博客
categories: 转载
cover: 'https://lizi.s3.bitiful.net/2024/12/5-20241227023044-b1cr03a.png'
date: 2024-12-27 00:00:00
---

> 转自大佬[ivelisya’s blog](https://www.smazyu.fun/杂项/2024安装hexo和next并部署到github和服务器最新教程.html)的博客
## 碎碎念

本来打算写点算法题​~~上文所说的题目~~​，结果被其他事情吸引了注意力。其实我之前也有过其他博客网站，但因为长期不维护，导致数据丢失​~~其实是我懒得备份~~​。这个博客现在部署在GitHub Pages上，​~~github不倒，网站不灭~~​，既省钱又稳定，但也有一些小问题。比如由于中国的gfw，GitHub的访问经常会受到限制，导致国内的小伙伴有时无法顺畅访问\>\_\<。~~虽然我的博客里也没写啥有用的东西~~但还是希望能有更多的人看到​~~或许我应该做做SEO？~~ ​✨

刚好，我手上有两台服务器，在服务器上使用git创建私有库，并利用git hooks进行自动化部署，每次上传到gitHub的同时，数据也会同步到服务器。具体的教程，下次有时间再更新，这篇文章暂时就作为一个~~占位符？~~ ​**占位文章！** ​，先在这里放着\>ᴗ\<

---

## 正文

**你能找到这篇文章，相信你已经成功部署了hexo至本地**

当然，为了照顾还没有部署的小伙伴

我在这里提供简短的教程 具体步骤请百度~~我觉得百度是世界上最大的广告站，到处都是广告~~ 也可以参考[Hexo官方文档](https://hexo.io/docs/)获取更详细的信息。

---

这是大佬的blog网站 [ivelisya’s blog](https://www.smazyu.fun/)

如果有人需要源码请在评论区留言喔~~其实是我想要多一点评论啦~~ 欢迎交流与分享！

![alt text](https://lizi.s3.bitiful.net/2024/12/5-20241227023044-b1cr03a.png)

---

### hexo本地部署教程~~超简化版~~

#### 1.下载安装基础运行程序。

* Git [https://git-scm.com/](https://git-scm.com/)
* Node.js [https://nodejs.org/en/](https://nodejs.org/en/)

#### 2.安装hexo

在你的node安装好并**配置环境变量**之后，会有一个npm命令

到你想要安装hexo的地方打开cmd

然后执行以下命令：

```
npm install -g hexo   # 进行安装
hexo init             # 初始化
```

到这里你已经完成了一半了~~是不是很简单~~

#### 3.安装主题 ~~毕竟原版不好看~~

这里我使用 next 主题

我安装的版本是 nexT v8

我更推荐的版本也是 nexT v8，原因的话就是老版本bug有点多

|年|版本号|仓库地址|
| ---------------| -----------| ----------|
|2014\~2017|v5|[GitHub 仓库](https://github.com/iissnan/hexo-theme-next)|
|2018\~2019|v6\~v7|[GitHub 仓库](https://github.com/theme-next/hexo-theme-next)|
|2020|v8|[GitHub 仓库](https://github.com/next-theme/hexo-theme-next)|

安装主题就是站点根目录下直接 git clone 下来就可以了

`git clone https://github.com/next-theme/hexo-theme-next`​即可

* 打开站点配置文件，将 `theme`​ 那项改成 `theme: next`​

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
# 搜索
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
```

* 然后你就可以打开看看效果啦

```
hexo clean && hexo g && hexo s
```

```
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.
```

* 访问 [http://localhost:4000/](http://localhost:4000/) 即可查看本地部署效果

**现在你已经完成本地部署了**

### 部署至github page

* 然后到 GitHub 创建一个和你用户名相同的仓库，后面加 `.github.io`​

以我的 blog 为例，如下

![alt text](https://lizi.s3.bitiful.net/2024/12/1-20241227023044-7wa2rhl.png)

* 然后我们要实现免密登录，毕竟每次都输入密码也很麻烦

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

你也可以检查一下

```
git config user.name
git config user.email
```

* 然后 `Win + R`​ 打开 cmd，然后执行

```
ssh-keygen -t rsa -C yourEmail
```

例如

```
ssh-keygen -t rsa -C simazhangyu@gmail.com
```

* 然后你会发现 `c:\user\yourName`​ 下有一个 `.ssh`​ 目录，如果没有，打开隐藏文件夹，如下图

![alt text](https://lizi.s3.bitiful.net/2024/12/2-20241227023044-k7lh97l.png)

`.ssh`​ 文件夹下有 `id_rsa`​ 和 `id_rsa.pub`​，前一个是私钥，后一个是公钥

~~有兴趣的小伙伴可以了解一下 :~~​~~[ssh 公钥私钥原理](https://blog.csdn.net/weixin_38172229/article/details/130503742)~~ 推荐阅读：[SSH 密钥原理详解](https://www.ssh.com/ssh/keygen/)

我们需要将 `id_rsa.pub`​ 内的内容复制下来，然后进入 GitHub，点击 `Settings`​，进入 `SSH and GPG keys`​ 选项，点击 `New SSH key`​，然后填写 `Title: Blog`​，`Key:你刚刚复制的公钥`​

**到这里你已经完成了一大半啦**

* 进行配置

![alt text](https://lizi.s3.bitiful.net/2024/12/3-20241227023044-i24k9kt.png)

点开这个 `_config.yml`​

![alt text](https://lizi.s3.bitiful.net/2024/12/4-20241227023044-h279at3.png)

对这个 `deploy`​ 进行修改

```
deploy:
    type: git
    repo: git@github.com:你的github用户名/你的github用户名.github.io.git
    branch: main
```

确保 `deploy`​ 配置正确，可以参考[Hexo 部署文档](https://hexo.io/docs/deployment)

至此你就完成了部署到 `github page`​ 的全部教程

* `hexo clean && hexo g && hexo d`​ 解释  
  ​`hexo clean`​ 清理缓存  
  ​`hexo g`​ 生成文章  
  ​`hexo d`​ 推送到远端

使用 `hexo d`​ 推送试试吧，不出意外过一会你就可以访问 `https://你的仓库名`​ 来访问你的 blog 了，以我的 blog 为例，访问 `https://ivelisya.github.io`​ 即可

**现在你已经完成部署至 github page 上了**

### 数据保存

数据无价，辛辛苦苦写的 blog 数据丢失可就不好了

所以我建议在 GitHub 上面再开一个仓库来存放 blog 的源码

我建议是把 `node_modules`​ 这个文件夹也上传到仓库的，反正也不大，要是使用其它设备就可以直接 `git clone`​ 然后编写博客了，而不需要重新下载 `hexo`​ 这一步骤将加快部署速度。

默认大家都会使用 `git`​，所以仅提供部分代码

```
# 强制添加 node_modules 文件夹
git add -f node_modules/
# 提交更改
git commit -m "Add node_modules directory"
# 推送到远程仓库
git push
```

### 部署至远端服务器

部署在 GitHub Pages 上有一些小问题。比如由于中国的 gfw，GitHub 的访问经常会受到限制，导致国内的小伙伴有时无法顺畅访问。所以可以同步部署在远端服务器上。这样以后推送的时候就会同时推到 GitHub 和个人服务器上了。

#### 服务器设置

---

* 登录服务器，然后安装 `git`​ 和 `nginx`​  
  我使用的是 `root`​ 账户  
  服务器系统为 `ubuntu`​，`centos`​ 代码可能有所不同，请自行修改

  ```
  sudo apt update
  sudo apt upgrade
  sudo apt install git
  sudo apt install nginx
  ```
* 创建一个 git 裸库

  ```
  cd ~
  git init --bare blogit.git
  ```
* 使用 git hooks 进行自动化

  ```
  vim blogit.git/hooks/post-receive
  ```
* 填入以下代码

  ```
  #!/bin/sh
  git --work-tree=/var/www/html --git-dir=/root/blogit.git checkout -f
  ```
* 授予可执行权限

  ```
  chmod +x /root/blogit.git/hooks/post-receive
  ```
* 实现免密登录

  ```
  cd .ssh/
  vim authorized_keys
  ```

  将 `id_rsa.pub`​ 的内容填入，`id_rsa.pub`​ 位于

  ![alt text](https://lizi.s3.bitiful.net/2024/12/6-20241227023044-0613d68.png)

  如果没有请 `Win + R`​ 打开 cmd，然后执行

```
ssh-keygen -t rsa -C yourEmail
```

例如

```
ssh-keygen -t rsa -C simazhangyu@gmail.com
```

详细步骤请参考上文

**这里有一个问题**

服务器可能没有开 ssh 公钥登录，所以你可能需要手动设置

##### 服务器设置 - 开启 ssh 公钥认证

编辑 `/etc/ssh/sshd_config`​ 文件

![alt text](https://lizi.s3.bitiful.net/2024/12/7-20241227023044-q0knt5i.png)

如果这里是 `no`​，或者被 `#`​ 注释掉了，取消注释，设置成如图所示那样

然后执行

```
sudo systemctl restart ssh
```

#### 本地设置

修改配置文件，路径为

![alt text](https://lizi.s3.bitiful.net/2024/12/3-20241227023044-i24k9kt.png)

点开这个 `_config.yml`​

![alt text](https://lizi.s3.bitiful.net/2024/12/4-20241227023044-h279at3.png)[https://www.smazyu.fun/images/hexo_next/4.png](https://www.smazyu.fun/images/hexo_next/4.png "alt text")

对这个 `deploy`​ 进行修改

```
deploy:
    - type: git
        repo: https://github.com/Ivelisya/Ivelisya.github.io.git
        branch: main
    - type: git
        repo: root@yourServerIP:/root/blogit.git
        branch: master
```

确保两个 `deploy`​ 配置项都正确，可以参考[Hexo 多部署配置](https://hexo.io/docs/deployment#multiplex-deployment)

**到这里你就已经完成了所有教程了✨**

快使用 `hexo d`​ 推送试试吧！

```
#!/bin/sh
git --work-tree=/opt/1panel/apps/openresty/openresty/www/sites/blog.lizisheji.cn/index --git-dir=/root/blogit.git checkout -f

```


`hexo cl && hexo generate && hexo deploy`
