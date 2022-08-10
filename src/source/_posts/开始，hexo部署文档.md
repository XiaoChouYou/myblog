---
title: 开始，hexo部署文档
date: 2022-08-08 06:53:13
tags:
---


# 搭建过程
## linux基础环境（git+nodejs+Hexo）

### linux 虚拟机版本
```bash
root@userapp-ubuntu02:/data/app# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04 LTS
Release:	22.04
Codename:	jammy
```
### 更新源信息安装git
```bash
apt-get update
apt-get install git -y
```

<!-- more -->

### 安装nvm
```bash
mkdir -p /data/app
cd /data/app 
git clone https://github.com/creationix/nvm.git nvm
source /data/app/nvm/nvm.sh

cat "source /data/app/nvm/nvm.sh" >> ~/.bashrc
```
* 如果不能下载可以手动访问GitHub下载 https://github.com/nvm-sh/nvm

### 安装nodejs+hexo
```bash
nvm install node

npm install -g hexo-cli
```
### 初始化hexo
```bash
mkdir -p /data/app/blog
cd /data/app/blog
hexo init
```
### hexo静态部署+本地验证
```bash
hexo g
hexo s
```
* 根据提示网址验证是否成功 http://localhost:4000/


## 关联github

### 安装Git部署插件
```bash
npm install hexo-deployer-git --save
```

### 修改配置
```bash
cd /data/app/blog
vi _config.yml
```
修改内容
```yaml
deploy:
  type: git
  repository: https://{你的token}@github.com/{git用户名}/{git用户名}.github.io.git  #你的仓库地址
  branch: master
```
配置git账号以及邮箱
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

测试三个命令
```bash
hexo clean   #清除缓存文件 db.json 和已生成的静态文件 public
hexo g       #生成网站静态文件到默认设置的 public 文件夹(hexo generate 的缩写)
hexo d       #自动生成网站静态文件，并部署到设定的仓库(hexo deploy 的缩写)
```
* 完成以后，打开浏览器，输入 https://xxx.github.io 就可以打开你的网页了.

## 设置主题
### 下载 next 主题
```bash
cd /data/app/blog
git clone https://github.com/theme-next/hexo-theme-next themes/next

cd themes/next
rm -rf .git
```
### 设置blog选择 next 主题
```bash
cd /data/app/blog
vi _config.yml
```
修改如下
```bash
# Site
title: 枫叶苑  #标题
subtitle: ''
description: 选择有时候比努力更重要     #简介或者格言
keywords:
author: 枫叶     #作者
language: zh-CN     #主题语言
timezone: Asia/Shanghai    #中国的时区

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next   #主题改为next
```
* 主题语言主要是看你的themes/next/language中的简体中文是 zh-CN 还是 zh-Hans：

next主题有四种，
依次为Muse、Mist、Pisces、Gemini（反正我没看出来后两个有什么区别）：
Blog/themes/next/下的_config.yml,只要将你选的主题前的#删除就行了：
```bash
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini    #这是我选的主题
```

配置完成后回到blog目录部署生效
```bash
cd /data/app/blog
hexo clean
hexo g
hexo d
```

* 可以先使用hexo s 本地查看样式之后再进行hexo d 同步到github.
* 完成以后，打开浏览器，输入 https://xxx.github.io 就可以打开你的最新网页了.

## 增加文档
### 样例文档
```bash
hexo new "开始，hexo部署文档"
```
将总结的md文档写入 “开始，hexo部署文档”
```bash
cat > /data/app/blog/source/_posts/开始，hexo部署文档.md <<EOF
XXX
XXXX
EOF
```
