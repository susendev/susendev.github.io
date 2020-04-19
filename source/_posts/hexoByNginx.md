---
title: 部署 hexo 到 nginx
date: 2020-04-19 21:25:13
tags:
---
本来博客是使用 GitHub pages ，但近些日子访问贼慢，刚好手里有一台小机器，当然是要用起来（折腾就对了🐶🐶🐶）。
<!-- more -->
## 前置条件

已购买 vps 和域名，按需备案。

ssh 登录远程服务器，以 CentOS 为例：

## git

- 安装

  ```shell
  yum install git
  ```

- 测试

  ```shell
   git --version
  ```

  输出类似下面的结果则表示安装成功

  ```shell
  git version 1.8.3.1
  ```

- 创建 git 用户

  ```shell
  adduser git
  ```

- 建立裸仓

  ```shell
  cd /home/git  # 进入 git 用户目录
  mkdir blog && chown -R git:git $_# 创建博客文件夹，，设置权限,作为 nginx web 目录
  mkdir projects && chown -R git:git $_ && cd $_  # 创建项目目录，设置权限并进入
  git init --bare hexo.git && chown -R git:git $_ # 创建博客裸仓，设置权限
  ```

- 添加 SSH Key，通过 ssh 链接仓库

  ```shell
  cd /home/git  # 回到 git 用户目录
  mkdir .ssh # 存放 ssh key
  ```

  在本地机器（写博客的电脑）上创建的 ssh 公钥（已有请忽略）

  ```shell
  ssh-keygen -o -t rsa -b 4096 -C "email@example.com" # 替换邮箱
  ```

  复制公钥

  ```shell
  pbcopy < ~/.ssh/id_rsa.pub
  ```

  在远程服务器，使用 vim 编辑文件，vim 用法请参考：

  ```shell
  vi /home/git/.ssh/authorized_keys
  ```

  按 `i` 进入编辑模式，粘贴公钥，按 `esc`，输入 `:wq` 保存并退出。

## hexo

- 配置发布选项

  修改 _config.yml （本级 hexo 配置文件）

  ```shell
  deploy:
    type: git
    repo: git@ip:/home/git/projects/hexo.git # ip 为服务器ip
    branch: master
  ```

- 自动部署（服务器）

  ```shell
  cd /home/git/projects/hexo.git/hooks # 进入 hook 目录
  mv post-update.sample post-update # 重命名 post-update
  vi post-update # vim 进行编辑
  ```

  按 `i` 进入编辑模式，在最后一行上面粘贴下面文字，按 `esc`，输入 `:wq` 保存并退出。

  ```shell
  git --work-tree=/home/git/blog --git-dir=/home/git/projects/hexo.git checkout -f
  ```

- 在本地 hexo 目录执行发布命令

  ```shell
  hexo g -d
  ```

- 查看服务器 blog 目录中是否有文件，如果没有请检查步骤是否错误。

- 源代码存放

  在我们服务器上存放的是变异后的文件，源文件我建议存放在 github 私有仓库。

## nginx

以 centos 为例：

- 安装

  ```shell
  yum install -y nginx
  ```

- 启动

  ```shell
  service nginx start
  ```

- 测试

  ```shell
  wget http://127.0.0.1
  ```

  可以正常下载 index.html 文件则说明启动成功。

- 配置

  ```shell
  vi /etc/nginx/nginx.conf
  ```

  把 `user nginx;` 修改为`user root;`

  server 按照修改两处配置：

  ```shell
  root /home/git/blog;
  location / {
  	  index index.html;
   }
  ```

  重启 nginx

  ```shell
  service nginx restart
  ```

  在浏览器键入你的服务器 ip 地址或域名，即可正常访问。

## https

使用 certbot 自动获取证书，参考 https://certbot.eff.org/

- 安装 Certbot

  ```shell
  sudo yum install certbot python2-certbot-nginx
  ```

- 获取证书并自动配置

  ```shell
  sudo certbot --nginx
  ```

  按照提示输入即可

- 自动续签

  ```shell
  echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
  ```

## 结束

至此，配置结束，开始写你的博客吧～
